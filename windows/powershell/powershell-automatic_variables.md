# PowerShell Automatic Variables


Notes on some of the more useful automatic vars:
* `$?` - success (`$true`) or failure (`$false`) of the last PowerShell command. Unfortunately gets obscured by running commands in `()` or through `Invoke-Command`.
* `$_` aka `$PSItem` - contains the current item from the pipeline.
* `$args` - array containing undeclared parameters.
* `$Error` - array of errors that have occurred, in reverse-chronological order. Run commands with `-ErrorAction` or `$ErrorActionPreference` set to `Ignore` to avoid populating this array.
* `$HOME` - full path to user's home dir
* `$Host` - represents current host application for PowerShell; use to get info about this application. Note that a remote session will effectively have a different host, which may not provide expected info (e.g. UI colors).
* `$LASTEXITCODE` - last exit status from a Windows command. Gets weird with PowerShell:
    - Always set by PowerShell script if script used `exit` command to exit
    - Not set by PowerShell script if run normally and terminated without `exit` command; beware that in this case it will still hold the last value it was set with
    - If PowerShell script was run through the `PowerShell` command, will be set to 0 if the script exited normally, 1 if the script exited by throwing an error
* `$MyInvocation` - populated for scripts, functions, and ScriptBlocks; holds information about how the currently running command was invoked
    - `$MyInvocation.MyCommand.Path` - path and filename for current script (if run from a file)
    - `$MyInvocation.MyCommand.Name` - name of current command (if named)
    - `$MyInvocation.PSScriptRoot` - only if caller is a script, full path to invoking script parent dir
    - `$MyInvocation.PSCommandPath` - only if caller is a script, full path and filename of invoking script
* `$PROFILE` - full path to profile for current user and PowerShell host
* `$PSBoundParameters` - dictionary of parameter values; present only if parameters are declared; only contains explicitly set parameters, not default values
* `$PSCmdlet` - represents the cmdlet or advanced function that is running
* `$PSCommandPath` - full path (including name) of the script being run
* `$PSDebugContext` - tracks debugging environment
* `$PSHOME` - path to the PowerShell install dir
* `$PSScriptRoot` - full path to running script's parent dir
* `$PWD` - present working dir
* `$StackTrace` - stack trace for the most recent error


Source: https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_automatic_variables
