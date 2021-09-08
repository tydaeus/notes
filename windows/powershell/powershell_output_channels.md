# PowerShell Output Channels

Sources:
* [about_output_streams](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_output_streams)
* [about_redirection](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_redirection)

PowerShell script output gets divided into different output streams, theoretically allowing the scripter and user to have fine-grained control over what gets displayed on screen, what gets output to a variable, what gets output to a file, and what gets ignored.

The streams are:

* 1 - Success
* 2 - Error
* 3 - Warning
* 4 - Verbose
* 5 - Debug
* 6 - Information
* '' - Progress

Each stream has different ways to write to it, and comes with its own quirks and caveats.


