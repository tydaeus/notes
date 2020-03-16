# Working with Scheduled Tasks in PowerShell

## Schedule a Task
PowerShell provides cmdlets that allow you to automate/script some of the functionality of the Windows Task Scheduler. Because of the complexity of the commands involved, it can help to build up the parameters as variables first.

The following template is derived from https://blogs.technet.microsoft.com/heyscriptingguy/2015/01/13/use-powershell-to-create-scheduled-tasks/.

``` PowerShell
# the action to be scheduled; executes PowerShell and specifies the command as part of its arguments
$action = New-ScheduledTaskAction -Execute 'Powershell.exe' -Argument '-NoProfile -WindowStyle Hidden -command "& {psScriptBlock}"'

# script execution variant
# recommend using this for all but the simplest actions, and will be easier if the script
# requires no parameters
$action = New-ScheduledTaskAction -Execute 'Powershell.exe' -WorkingDirectory 'C:\ScriptDir' -Argument '-NoProfile -WindowStyle Hidden -command ".\scriptName.ps1"'


# triggering condition used to actually schedule the task
$trigger = New-ScheduledTaskTrigger -Daily -At 9am

# at log on variant (assuming we want it to happen at the current user's logon)
$trigger = New-ScheduledTaskTrigger -AtLogOn -User $env:USERNAME

# actually register the action and trigger
# add param `-User "System"` to run as system, which allows the task to run with no-one logged in. Note that this will prevent user-specific operations from performing, such as displaying dialog boxes.
# add param `-Settings (New-ScheduledTaskSettingsSet -WakeToRun)` to wake the computer for the task
Register-ScheduledTask -Action $action -Trigger $trigger -TaskName "MyTask" -Description "My task that does stuff"
```

### Creating and Using Task Settings
To further customize task behavior, you need to specify task settings using the `-Settings` parameter with a ScheduledTaskSettingsSet. The easiest way to do this is by creating a new ScheduledTaskSettingsSet object with `New-ScheduledTaskSettingsSet`, using that commandlet's parameters to specify the settings. E.g.:

```PowerShell
# create the settings
$settings = New-ScheduledTaskSettingsSet -StartWhenAvailable

# register the task with the settings specified (assuming $action and $trigger have already been created)
Register-ScheduledTask -Action $action -Trigger $trigger -TaskName "MyTask" -Description "My task that does stuff" -Settings $settings

```
**Recommended**: most tasks should probably have settings created with `-StartWhenAvailable` specified, otherwise high system activity may result in your task being skipped.




## Get Info About Scheduled Tasks
Run `taskschd.msc` for the GUI interface, or see below for PowerShell options.

List all scheduled tasks: `Get-ScheduledTask`

Get info about a named scheduled task: `Get-ScheduledTask $taskName | Get-ScheduledTaskInfo` where `$taskName` is a string containing the value of -TaskName used during scheduling.

## Unregister a Scheduled Task
Use `Unregister-ScheduledTask -TaskName $taskName` to unregister the named task.

## Stop Running Instances
Use `Stop-ScheduledTask -TaskName $taskName` to stop all running instances of a task, e.g. if a task instance has locked up
