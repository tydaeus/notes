# Common PowerShell Tasks

## Checking PowerShell Version
Use `$PSVersionTable.PSVersion` to check the version of PowerShell currently running. PowerShell 1.0 does not have this variable.

A `#Require` statement can be added to enforce minimum PowerShell version for the script, e.g. `#Requires -Version 5.0`. This must be the first item on a line, but can appear on any line.

## Get Information About Currently Running Script
The following automatic variables are populated with information about the currently running script. Note that these variables are populated *only* while running a script, and may also remain unpopulated under certain other conditions (e.g. invoking a script by reading its contents). If these variables are not populated, they will default to null, which will result in `C:\` acting as path value.

* `$PSScriptRoot` - full path to script directory
* `$PSCommandPath` - full path to script


## Set Constant
``` powershell
# ReadOnly can be removed via Remove-Variable with the -Force parameter set
Set-Variable varName -option ReadOnly -value $Value

# Constant cannot be removed
Set-Variable varName -option Constant -value $Value

```



## Wait for a Specified Time
Use the `Start-Sleep` command to cause the PowerShell session to sleep for a specified period, using either the `-Seconds` or `-Milliseconds` parameter, e.g. `Start-Sleep -Seconds 1.5`.

## Regex Matching

### The `-match` Operator
Use the `-match` operator to check whether a string(s) matches a regex. E.g. `$str -match $regex` will return a boolean indicating whether the string contained in `$str` matches the regex contained in (string variable) `$regex`.

If the first operand is a collection of strings, a collection of the matching strings will be returned.

The `$Matches` automatic Hashtable variable will be populated with the first match for each regex grouping if `-match` returns true, if matching a single string. `$Matches` is not updated if matching a collection.

The `-notmatch` operator performs the negation equivalent to `-not ($str -match $regex)`, and populates `$Matches` if matches were present.

### The `-replace` Operator
(notes based on https://vexx32.github.io/2019/03/20/PowerShell-Replace-Operator/)

Use the `-replace` operator to perform regex substitution on a string; syntax is `<STRING> -replace <REGEX>,<REPLACEMENT>`. All places where `<STRING>` matches `<REGEX>` will be replaced with `<REPLACEMENT>`.

`<REPLACEMENT>` can use standard regex numbered replacement groups, e.g. `'foo(1234,56789)' -replace '^.*\((\d+),.*$','$1'` results in `'1234'` (the whole string gets matched, but the first parenthetical group is the only piece kept in the replacement string). The `$0` replacement group represents the entire matched string. Be aware that the `$` must be escaped so that PowerShell doesn't attemt to interpret it as a variable reference.

A name can be specified for a replacement group in format `?<Name>` at the beginning of the replacement sequence, e.g. `'foo(1234,56789)' -replace '^.*\((?<Number>\d+),.*$','${Number}'`. Note that the name must be wrapped in `{}` in the replacement string.

### Regex Type
The `[regex]` object type can be used as an alternative to the `-match` operator. Use `[regex]::Matches($str, $regex)` to retrieve a collection of matches.

### `Regex.Escape` method
Use the `Escape` static method to escape all regex characters in a string. `[Regex]::Escape($string)`.



## File System Operations

### Copy Files

#### `Copy-Item`
Copy 1 file: `Copy-Item $SourcePath -Destination $DestinationPath`

Copy a directory, recursively: `Copy-Item $SourcePath -Destination $DestinationPath -Recurse`

Best Practice: Use the `-LiteralPath` param instead of the default `-Path` param to specify source if you don't intend to use PowerShell filename wildcards (`*[]`) in it.

#### `Start-BitsTransfer`
The BitsTransfer module provides network copying and feedback enhancements (e.g. an automatic progress bar). Note that this operation operates on a single file, so multiple file copy and directory structure copy have to be managed separately.

To copy a single file:

```PowerShell
Start-BitsTransfer -Source $sourceFile -Destination $targetFile -TransferType Download -Description "Copy $fileName" -DisplayName "Copy $fileName"
```



### Delete Files
Delete all files in dir: `Remove-Item $TargetDir -Force -Recurse`. Note that the recurse switch has issues, so you may be better off with `Get-ChildItem $TargetDir -Recurse | Remove-Item -Force -Recurse`.

As a function:
```PowerShell
function Remove-ChildItems {
    param(
        $TargetDir
    )

    Get-ChildItem $TargetDir -Recurse | Remove-Item -Force -Recurse
}
```


### Unzip File
In PowerShell 5+: `Expand-Archive $SourcePath -DestinationPath $DestinationPath`. If `-DestinationPath` is omitted, will extract to PWD.

### Make Dir
The `md` alias is probably the quicker way to create a dir, but you'll probably want to pipe its output to null if using in a script: `md $DirPath | Out-Null`. Use the `-Force` switch param if you want to create a dir that may already exist.

## Pass Function Call
Use `Get-Item "function:$FunctionName"` to get a reference to the function named in `$FunctionName`. Access its script block to invoke it via `(Get-Item "function:$FunctionName").ScriptBlock.Invoke($argArr)`.

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


## Working with Dates
Cast a string to a date via `[dateTime]"2018/11/23"`. A variety of formats are automatically recognized.

## Running PowerShell from Cmd
For "security" reasons, PowerShell scripts cannot be launched by double-clicking them. This can be easily bypassed, however, by launching PowerShell commands or scripts from a `.cmd` script.

Run a script:
```cmd
powershell -executionpolicy remotesigned -File ".\MyPowerShellScript.ps1" <args>
```
Note that all text after `-File` will be interpreted as part of the filepath or parameters for the script. Passing `-` as the value for the parameter will result in reading `stdin` as the file.

Run a command or series of commands as though typed at the PowerShell prompt:
```cmd
powershell -Command Do-Something -Param1 param1Value
```
Note that all text after `-Command` will be interpreted as part of the command.

### Self-Launching PowerShell Script
You can also embed a PowerShell script within a `.cmd` script by starting the `.cmd` script with the below snippet. Note, however, that because this relies on running the PowerShell script as a series of commands, some of the PowerShell automatic variables will not be automatically be populated.

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
    - Recommended: Use `(New-Guid).GetHashCode()` to generate a nearly unique id

``` PowerShell
$percentComplete = [Math]::Floor(($i / $totalThings) * 1000) / 10
Write-Progress -Activity "Doing the thing: $percentComplete% Complete" -PercentComplete $percentComplete

# after loop
Write-Progress -Activity "Doing the thing: Complete" -Completed
```
