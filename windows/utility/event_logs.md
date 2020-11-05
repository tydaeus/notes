# Windows Event Logs
Windows event logs can be view in event viewer. Use command `eventvwr` to launch event viewer.

The `wevtutil` cmdline utility can be used to export or manipulate the event logs from the command prompt. See https://michlstechblog.info/blog/windows-export-eventlog-from-command-line/ for an intro, or https://ss64.com/nt/wevtutil.html for reference.

The `Get-WinEvent` (or the deprecated `Get-EventLog`) PowerShell cmdlet can be used to retrieve event logs from the commandline or from within a script as PowerShell objects or text.