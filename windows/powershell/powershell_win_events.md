# Working with Windows Event Logs in PowerShell
PowerShell provides the `Get-WinEvent` cmdlet for retrieving Windows event logs.

Sample usage:

``` PowerShell
# Get all Application event log events
Get-WinEvent -LogName 'Application'
```

Getting all events for a particular log may be performance intensive. For simple queries, the `-MaxEvents` parameter can set a simple cap. For detailed queries, invocations are provided that allow for filtering; see the Microsoft documentation for details.

Depending on which logs you're retrieving from, log access may be denied; setting `-ErrorAction 'SilentlyContinue'` or `-ErrorAction 'Ignore'` may be desirable.

## Filtering with `-FilterHashTable`
The `-FilterHashTable` parameter allows you to provide a HashTable defining specific filtering criteria:

This HashTable supports keys:
* LogName=<String[]>
* ProviderName=<String[]>
    - must be providers defined on the system
    - if the array is longer than supported by Get-WinEvent (22 is max length found in testing), Get-WinEvent will throw an error indicating that no matching events were found
    - asterisk wild-carding supported
* Path=<String[]>
    - for querying log files
* Keywords=<Long[]>
* ID=<Int32[]>
* Level=<Int32[]>
    - values based on https://learn.microsoft.com/en-us/dotnet/api/system.diagnostics.eventing.reader.standardeventlevel
* StartTime=<DateTime>
* EndTime=<DateTime>
* UserID=<SID>
* Data=<String[]>

e.g.: `Get-WinEvent -FilterHashTable @{ 'LogName' = 'Application', 'ProviderName' = ' MyProvider', 'StartTime' = [DateTime]'2022-11-05' }`