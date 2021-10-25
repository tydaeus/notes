# Windows Event Logs
Windows event logs can be view in event viewer. Use command `eventvwr` to launch event viewer.

The `wevtutil` cmdline utility can be used to export or manipulate the event logs from the command prompt. See https://ss64.com/nt/wevtutil.html for reference and examples.

The `Get-WinEvent` (or the deprecated `Get-EventLog`) PowerShell cmdlet can be used to retrieve event logs from the commandline or from within a script as PowerShell objects or text.

## Export via Get-WinEvent
``` PowerShell
# ErrorAction set to SilentlyContinue because some logs may be restricted due to privileges
# Be aware that this gets the whole log, which can be quite large if piped directly into an output file; filtering before via XPath query or after via Where-Object is preferred.
$events = Get-WinEvent -LogName 'Application' -ErrorAction 'SilentlyContinue'

# post-filter to events from 10-25 only
$begin = [DateTime]'2021-10-25 00:00:00'
$end = [DateTime]'2021-10-25 11:59:59'
$timeFilteredEvents = $events |
     Where-Object { ($_.TimeCreated -ge $begin) -and ($_.TimeCreated -le $end) }


```

## Export via `wevtutil`
Export the application log via `wevtutil`:

``` cmd
WEVTUtil export-log Application C:\temp\application_events.evtx

:: archive the event log to include display data; this appears to only work from within the local folder
wevtutil al .\application_events.evtx
:: be sure to include the generated LocaleMetaData folder; this must be in parent folder to be found
```