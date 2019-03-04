# Working with Scheduled Tasks in PowerShell

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

## Get Info About Scheduled Task
Get info about a named scheduled task: `Get-ScheduledTask $taskName | Get-ScheduledTaskInfo` where `$taskName` is a string containing the value of -TaskName used during scheduling.

## Unregister a Scheduled Task
Use `Unregister-ScheduledTask -TaskName $taskName` to unregister the named task.
