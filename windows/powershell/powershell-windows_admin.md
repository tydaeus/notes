# PowerShell for Windows Admin

## Performance Monitoring with `Get-Counter`
The `Get-Counter` cmdlet provides robust performance sampling functionality, but is definitely a little complex to use. https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.diagnostics/get-counter

To use `Get-Counter` to retrieve performance data, you must specify one or more "Counter" names to query. Use `Get-Counter -ListSet *` to list all counter sets.

Running `Get-Counter` on a list of counters will return a single object whose `CounterSamples` property will be the aggregated results of all queried counters. Each CounterSample will include a `Path` property specifying the query that produced the counter and a `CookedValue` with the meaningful value for the counter.

## System data from CIM
Retrieve data through the Windows Common Information Model using `Get-CimInstance`: https://learn.microsoft.com/en-us/powershell/module/cimcmdlets/get-ciminstance.

Use `Get-CimClass` to list all available CIM classes, or search available classes with `Get-CimClass -ClassName $className`; asterisk wildcarding supported.

`Get-CIMInstance -ClassName win32_process` get information about running processes.

`Get-CIMInstance Win32_OperatingSystem` gets OS-level information, e.g. available and free RAM.
