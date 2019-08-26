# Power Configuration
Many windows power configuration operations can be controlled via commandline/script.

## `powercfg /change`
Use `powercfg /change` to modify power save settings.

Specify one of the following:

* `monitor-timeout-ac`
* `monitor-timeout-dc`
* `disk-timeout-ac`
* `disk-timeout-dc`
* `standby-timeout-ac`
* `standby-timeout-dc`
* `hibernate-timeout-ac`
* `hibernate-timeout-dc`

`AC` options control power when plugged in, `DC` while on battery power.

E.g. `powercfg /change monitor-timeout-ac 5` will set the monitor to sleep after 5 minutes when plugged into the wall.

See https://docs.microsoft.com/en-us/windows-hardware/design/device-experiences/powercfg-command-line-options#option_change for fuller documentation.

## Disable hybrid sleep/hibernation
Hybrid sleep/hibernation can potentially interfere with wakeup scripts. Use `powercfg -h off` to disable hybrid sleep if you experience issues with wakeup scripts.

## Put computer to sleep
```cmd
Rundll32.exe Powrprof.dll,SetSuspendState Sleep
```

## Wake the computer
Any task that is configured to be allowed to wake the computer will do so when it is run. This is configured under the `Conditions` tab of Task Scheduler. This can be configured when scheduling in PowerShell by passing a `-Settings` parameter to `Register-ScheduledTask` containing ScheduledTaskSettingsSet object created via `New-ScheduledTaskSettingsSet -WakeToRun`.
