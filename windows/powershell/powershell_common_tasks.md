# Common PowerShell Tasks

## Checking PowerShell Version
Use `$PSVersionTable.PSVersion` to check the version of PowerShell currently running. PowerShell 1.0 does not have this variable.

A `#Require` statement can be added to enforce minimum PowerShell version for the script, e.g. `#Requires -Version 5.0`. This must be the first item on a line, but can appear on any line.

## Get Script Directory
In PowerShell 3.0+, automatic variable `$PSScriptRoot` will contain the dir of the current script. Note that this variable is populated *only* while running a script, and may also remain unpopulated under certain other conditions (e.g. invoking a script by reading its contents). If this variable is not populated, it will default to null, which will result in `C:\` acting as its value.

## Using .NET Libraries
You can use static methods from .NET libraries by referencing their FullName in  brackets and adding the method name after double colons: `[System.IO.Path]::GetExension("Filename.ext")`.

Note that `System` can be dropped from the FullName: `[IO.Path]::GetExension("Filename.ext")`.

In PowerShell 5.0+, the `using` statement is available to allow C#-like easy access to namespaces. E.g. `using namespace Long.NameSpace.Example.Name`.

## Set Constant
``` powershell
# ReadOnly can be removed via Remove-Variable with the -Force parameter set
Set-Variable varName -option ReadOnly -value $Value

# Constant cannot be removed
Set-Variable varName -option Constant -value $Value

```
## File System Operations

### Copy Files
Copy 1 file: `Copy-Item $SourcePath -Destination $DestinationPath`

### Delete Files
Delete all files in dir: `Remove-Item $TargetDir -Force -Recurse`

### Unzip File
In PowerShell 5+: `Expand-Archive $SourcePath -DestinationPath $DestinationPath`. If `-DestinationPath` is omitted, will extract to PWD.

## Output to Null
Because any output from a line that isn't explicitly passed to a null-output command or stored in a variable is treated as an output from your script, you will probably wish to write your output to null sometimes. To do this, pipe to `Out-Null`.

## Get Type of Variable
Use the `GetType` method to determine the type of a variable: `$myVar.GetType()`.

## Join Collection
Use the `-Join` operator to join a collection.

```powershell
# join with no delimiter
-Join $myArr

# join with newline as delimiter
$myArr -Join "`n"
```

## Write a File

### Encoding
For Microsoft-known reasons, PowerShell output and redirection default to UTF-16 encoding with a Byte-Order Mark. This can be problematic.

This default can be changed by setting `$PSDefaultParameterValues['Out-File:Encoding'] = 'utf8'`. In PowerShell 3.0+, this modifies behavior of Out-File only. In PowerShell 5.1+, this also modifies the behavior of the redirect operators.

### Redirection
Redirection can be used to write to files: `$myString > $filePath`. Note, however, that this encodes the file as UTF-16 by default.

Redirection can also be used to append to files: `$myString >> $filePath`.

### Out-File
The `Out-File` cmdlet allows writing to files with some options. `$myString | Out-File $MyPath`.

This can be used to change encoding to UTF-8: `$myString | Out-File -Encoding "UTF8" $MyPath`. However, the Byte-Order Mark (`ï»¿`) will be written as the first several characters, which may result in incompatibility.

### `File::WriteAllLines`
Use .NET's `File` class static `WriteAllLines` method: `[System.IO.File]::WriteAllLines($MyPath, $MyString)`. In PowerShell 4.0+, this will default to UTF 8 with no Byte-Order Mark.

In PowerShell 3.0, use `[System.IO.File]::WriteAllLines($MyPath, $MyString, [System.Text.UTF8Encoding]::new($False))` to explicitly indicate UTF-8 with no BOM.

## Read a File

### Line-by-Line: `Get-Content`
Use `Get-Content $filePath` to retrieve a file's content as a collection of lines. Pipe this into `ForEach-Object` or use in a `foreach` loop to iterate through the lines.

`Get-Content` has known performance issues on large files, because it must read through the entire file before returning.

### Line-by-Line: `System.IO.File::ReadLines`
Use `[System.IO.File]::ReadLines($filePath)` to access .Net's `File.ReadLines` function to retrieve a file's content line by line. Pipe this into `ForEach-Object` or use in a `foreach` loop to iterate through the lines.

### XML Parsing
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

## Check on Files at Path
Use the `Test-Path` cmdlet to inspect file structures. Use `Get-Help Test-Path` for full help.

### Check if Path Exists
`Test-Path -Path $fullPath`

### Check if Path is Valid
`Test-Path -Path $fullPath -IsValid`

### Check if Path is Dir
`Test-Path -Path $fullPath -PathType Container`

### Check if Path is File
`Test-Path -Path $fullPath -PathType leaf`

## Send an Email
Use the `Send-MailMessage` cmdlet to send an email: `Send-MailMessage -From $fromAddress -To $toAddress -CC $ccAddress -Subject $Subject -Body $Body -SmtpServer $SMTPServer - port $SMTPPort -Attachments $Attachment`. You may wish to add the `-UseSsl` parameter to ensure SSL encryption, and setting `-Credential (Get-Credential)` will ensure credentials are prompted for.

## Schedule a Task
PowerShell provides cmdlets that allow you to automate/script some of the functionality of the Windows Task Scheduler. Because of the complexity of the commands involved, it can help to build up the parameters as variables first.

The following template is derived from https://blogs.technet.microsoft.com/heyscriptingguy/2015/01/13/use-powershell-to-create-scheduled-tasks/.

```
# the action to be scheduled; executes PowerShell and specifies the command as part of its arguments
$action = New-ScheduledTaskAction -Execute 'Powershell.exe' -Argument '-NoProfile -WindowStyle Hidden -command "& {psScriptBlock}"'

# script execution variant
# recommend using this for all but the simplest actions, and will be easier if the script
# requires no parameters
$action = New-ScheduledTaskAction -Execute 'Powershell.exe' -WorkingDirectory 'C:\ScriptDir' -Argument '-NoProfile -WindowStyle Hidden -command ".\scriptName.ps1"'


# triggering condition used to actually schedule the task
$trigger = New-ScheduledTaskTrigger -Daily -At 9am

# actually register the action and trigger
# add param `-User "System"` to run as system, which allows the task to run with no-one
# logged in
Register-ScheduledTask -Action $action -Trigger $trigger -TaskName "MyTask" -Description "My task that does stuff"
```

### Get Info About Scheduled Task
`Get-ScheduledTask $taskName | Get-ScheduledTaskInfo` where `$taskName` is a string containing the value of -TaskName used during scheduling.


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
