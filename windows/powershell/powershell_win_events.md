# Working with Windows Event Logs in PowerShell
PowerShell provides the `Get-WinEvent` cmdlet for retrieving Windows event logs.

Sample usage:

``` PowerShell
# Get all Application event log events
Get-WinEvent -LogName 'Application'
```

Getting all events for a particular log may be performance intensive. For simple queries, the `-MaxEvents` parameter can set a simple cap. For detailed queries, invocations are provided that allow for filtering; see the Microsoft documentation for details. <!-- FUTURE: provide examples -->

Depending on which logs you're retrieving from, log access may be denied; setting `-ErrorAction 'SilentlyContinue'` may be desirable.