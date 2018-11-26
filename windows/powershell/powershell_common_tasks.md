# Common PowerShell Tasks

## Get Script Directory
In PowerShell 3.0+, automatic variable `$PSScriptRoot` will contain the dir of the current script.

## Using .NET Libraries
You can use static methods from .NET libraries by referencing their FullName in  brackets and adding the method name after double colons: `[System.IO.Path]::GetExension("Filename.ext")`.

## Set Constant
``` powershell
# ReadOnly can be removed via Remove-Variable with the -Force parameter set
Set-Variable varName -option ReadOnly -value $Value

# Constant cannot be removed
Set-Variable varName -option Constant -value $Value

```

## Get Type of Variable
Use the `GetType` method to determine the type of a variable: `$myVar.GetType()`.

## Read a File

### Line-by-Line: `Get-Content`
Use `Get-Content $filePath` to retrieve a file's content as a collection of lines. Pipe this into `ForEach-Object` or use in a `foreach` loop to iterate through the lines.

`Get-Content` has known performance issues on large files, because it must read through the entire file before returning.

### Line-by-Line: `System.IO.File::ReadLines`
Use `[System.IO.File]::ReadLines($filePath)` to access .Net's `File.ReadLines` function to retrieve a file's content line by line. Pipe this into `ForEach-Object` or use in a `foreach` loop to iterate through the lines.

### XML Parsing
Retrieve an xml file via `[xml]$xmlFile = Get-Content $xmlFilePath`. "Dot-Walk" on the object to retrieve the contents of all tags of a specified nesting and type. E.g. `$xmlFile.xml.TagLev1.TagLev2` will retrieve the contents of all TagLev2 tags that are contained within TagLev1 tags which are contained within an xml tag. Note that not all xml files use the `xml` tag as a base-level tag.

**Warning**: a "null" object will be returned for each instance of the parent tag that does not contain the desired child tag (e.g. for each `TagLev1` tag that doesn't contain a `TagLev2`).

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
$action = New-ScheduledTaskAction -Execute 'Powershell.exe' -WorkingDirectory 'C:\ScriptDir' -Argument '-NoProfile -WindowStyle Hidden -command ".\scriptName.ps1"'


# triggering condition used to actually schedule the task
$trigger = New-ScheduledTaskTrigger -Daily -At 9am

# actually register the action and trigger
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
