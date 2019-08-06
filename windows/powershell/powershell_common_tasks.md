# Common PowerShell Tasks

## Checking PowerShell Version
Use `$PSVersionTable.PSVersion` to check the version of PowerShell currently running. PowerShell 1.0 does not have this variable.

A `#Require` statement can be added to enforce minimum PowerShell version for the script, e.g. `#Requires -Version 5.0`. This must be the first item on a line, but can appear on any line.

## Get Script Directory
In PowerShell 3.0+, automatic variable `$PSScriptRoot` will contain the dir of the current script. Note that this variable is populated *only* while running a script, and may also remain unpopulated under certain other conditions (e.g. invoking a script by reading its contents). If this variable is not populated, it will default to null, which will result in `C:\` acting as its value.

## Set Constant
``` powershell
# ReadOnly can be removed via Remove-Variable with the -Force parameter set
Set-Variable varName -option ReadOnly -value $Value

# Constant cannot be removed
Set-Variable varName -option Constant -value $Value

```



## Regex Matching

### The `-match` Operator
Use the `-match` operator to check whether a string(s) matches a regex. E.g. `$str -match $regex` will return a boolean indicating whether the string contained in `$str` matches the regex contained in (string variable) `$regex`.

If the first operand is a collection of strings, a collection of the matching strings will be returned.

The `$Matches` automatic Hashtable variable will be populated with the first match for each regex grouping if `-match` returns true, if matching a single string. `$Matches` is not updated if matching a collection.

The `-notmatch` operator performs the negation equivalent to `-not ($str -match $regex)`, and populates `$Matches` if matches were present.

### Regex Type
The `[regex]` object type can be used as an alternative to the `-match` operator. Use `[regex]::Matches($str, $regex)` to retrieve a collection of matches.



## File System Operations

### Copy Files
Copy 1 file: `Copy-Item $SourcePath -Destination $DestinationPath`

### Delete Files
Delete all files in dir: `Remove-Item $TargetDir -Force -Recurse`

### Unzip File
In PowerShell 5+: `Expand-Archive $SourcePath -DestinationPath $DestinationPath`. If `-DestinationPath` is omitted, will extract to PWD.

### Make Dir
The `md` alias is probably the quicker way to create a dir, but you'll probably want to pipe its output to null if using in a script: `md $DirPath | Out-Null`. Use the `-Force` switch param if you want to create a dir that may already exist.

## Pass Function Call
Use `Get-Item "function:$FunctionName"` to get a reference to the function named in `$FunctionName`. Access its script block to invoke it via `(Get-Item "function:$FunctionName").ScriptBlock.Invoke($argArr)`.

## Get Type of Variable
Use the `GetType` method to determine the type of a variable: `$myVar.GetType()`. This operation is not null safe, so perform a null check first.

## Join Collection
Use the `-Join` operator to join a collection.

```powershell
# join with no delimiter
-Join $myArr

# join with newline as delimiter
$myArr -Join "`n"
```

## XML Parsing
Retrieve an xml file via `[xml]$xmlFile = Get-Content $xmlFilePath`. "Dot-Walk" on the object to retrieve the contents of all tags of a specified nesting and type. E.g. `$xmlFile.xml.TagLev1.TagLev2` will retrieve the contents of all TagLev2 tags that are contained within TagLev1 tags which are contained within an xml tag. Note that not all xml files use the `xml` tag as a base-level tag.

**Warning**: a "null" object will be returned for each instance of the parent tag that does not contain the desired child tag (e.g. for each `TagLev1` tag that doesn't contain a `TagLev2`).

## Working with JSON

### Reading JSON
Use `ConvertFrom-Json` to convert a string into a PSCustomObject, similar to `JSON.Parse`. Note that this works only with a string, not a string array, so you'll need to `-Join` the string[] if reading from a file.

All together:
```powershell
$resultObject = (Get-Content $filePath) -Join "`n" | ConvertFrom-Json
```

### Writing JSON
Use `ConvertTo-Json` to convert an object into JSON, similar to `JSON.Stringify`.

*Note*: this cmdlet has known inconsistencies with array conversion, due to the way arrays get passed through the object pipeline; this results in arrays sometimes getting converted into an object with `count` and `values` properties, rather than into a JSON array. To prevent this, use the following in your script prior to performing the conversion:

```powershell
# workaround for ConvertTo-Json issue with array conversion
try {
    Remove-TypeData System.Array
}
catch {
    # do nothing if System.Array TypeData has already been removed
}
```

## Get Contents of a Directory
`Get-ChildItem $dirPath`. Use the `-Recurse` parameter for recursive traversal, and the `-Force` parameter to include hidden and system files.

### Traverse Directory Recursively

#### Piping and Filtering to get Specific FileNames
`Get-ChildItem $dirPath -Recurse` for all files.

`Get-ChildItem $dirPath -Recurse | Where-Object {$_.Extension -eq $ext}` to include only the specified extension.

`Get-ChildItem $dirPath -Recurse | Where-Object {$_.Extension -eq $ext} | select FullName` to convert to a list of all matching files, with their full paths.

This pattern can be modified to select based on other features of the file, too.

#### Via Custom Function, Allowing Advanced Processing
This option can be adapted to exclude some directories from the recursion. Adapted from https://stackoverflow.com/a/10585857/2939139:

```
function GetFiles($path = $pwd)
{
    foreach ($item in Get-ChildItem $path)
    {
        # return the item; substitute with other processing, if desired
        $item

        if (Test-Path $item.FullName -PathType Container)
        {
            GetFiles $item.FullName
        }
    }
}
```

See the original answer for a more elaborate solution that supports piping.


## Send an Email
Use the `Send-MailMessage` cmdlet to send an email: `Send-MailMessage -From $fromAddress -To $toAddress -CC $ccAddress -Subject $Subject -Body $Body -SmtpServer $SMTPServer - port $SMTPPort -Attachments $Attachment`. You may wish to add the `-UseSsl` parameter to ensure SSL encryption, and setting `-Credential (Get-Credential)` will ensure credentials are prompted for.

## Display a Dialog Box

### via Windows Script Host
From https://blogs.technet.microsoft.com/heyscriptingguy/2014/04/04/powertip-use-powershell-to-display-pop-up-window/, API documented in https://docs.microsoft.com/en-us/previous-versions/windows/internet-explorer/ie-developer/windows-scripting/x83z1d9f(v=vs.84).

```
# 0 causes box to display until an option is clicked
$nSecondsToWait = 0

# Only option is "Ok"
$dialogType = 0x0

$wshell = New-Object -ComObject Wscript.Shell
$buttonClicked = $wshell.Popup("Body Text",$nSecondsToWait,"Title",$dialogType)
```

## Working with Dates
Cast a string to a date via `[dateTime]"2018/11/23"`. A variety of formats are automatically recognized.

## Self-Launching PowerShell Script
For "security" reasons, PowerShell scripts cannot be launched by double-clicking them. This is easily bypassed however, by embedding the PowerShell script inside a `.cmd` script that tells PowerShell to run it.

Start the `.cmd` script with this snippet:
``` cmd
<# : batch script
@echo off
setLocal
powershell -executionpolicy remotesigned -Command "Invoke-Expression $($ScriptHome = '%~dp0'; [System.IO.File]::ReadAllText('%~dpf0'))"
set ERRCODE=%ERRORLEVEL%
exit /b %ERRCODE%
#>
# PowerShell script begins here

# Manually populate $PSScriptRoot, due to batch bootstrap limitations
$PSScriptRoot = $ScriptHome
```

## Progress Bar
Use the `Write-Progress` cmdlet to display a progress bar.

* `-Activity` - text title for bar; updated with every call
* `-PercentComplete` - how full the bar should be, out of 100
* `-Completed` - pass this switch to hide the bar; `-Activity` must be specified but will not be used
* `-Id` - numeric identifier for bar; not necessary unless displaying multiple bars

``` PowerShell
$percentComplete = [Math]::Floor(($i / $totalThings) * 1000) / 10
Write-Progress -Activity "Doing the thing: $percentComplete% Complete" -PercentComplete $percentComplete

# after loop
Write-Progress -Activity "Doing the thing: Complete" -Completed
```
