# Windows Event Logs
Windows event logs can be view in event viewer. Use command `eventvwr` to launch event viewer.

The `wevtutil` cmdline utility can be used to export or manipulate the event logs from the command prompt. See https://ss64.com/nt/wevtutil.html for reference and examples.

The `Get-WinEvent` (or the deprecated `Get-EventLog`) PowerShell cmdlet can be used to retrieve event logs from the commandline or from within a script as PowerShell objects or text.

When saving events from Event Viewer for viewing on another computer, you need to select the "Display information for these languages" option (and specify desired language(s)), and then keep the LocaleMetaData folder that will be generated in the parent folder alongside the event logs. Without this option selected, events may not include readable messages if viewed on a computer that doesn't have the software that generated the event installed.

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

### Useful Object Properties
The following event object properties can be useful for examining and/or filtering them
* `.TimeCreated` - DateTime when the event was created
* `.LevelDisplayName` - level at which event was logged
* `.ProviderName` - indicates what app logged the event (corresponds to the "Source" field in event viewer)
* `.Message` - description of the event



## Export via `wevtutil`
Export the application log via `wevtutil`:

``` cmd
WEVTUtil export-log Application C:\temp\application_events.evtx

:: archive the event log to include display data; this appears to only work from within the local folder
wevtutil al .\application_events.evtx
:: be sure to include the generated LocaleMetaData folder; this must be in parent folder to be found
```