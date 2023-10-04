# PowerShell Output Channels

Sources:
* [about_output_streams](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_output_streams)
* [about_redirection](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_redirection)

PowerShell script output gets divided into different output streams, theoretically allowing the scripter and user to have fine-grained control over what gets displayed on screen, what gets output to a variable, what gets output to a file, and what gets ignored.

The streams are:

* 1 - Success aka Out
* 2 - Error
* 3 - Warning
* 4 - Verbose
* 5 - Debug
* 6 - Information
* '' - Progress

Each stream other than Progress has a numeric identifier; only the numbered streams can be redirected.

Each stream has different ways to write to it, and comes with its own quirks and caveats.

Success is the stream used for output, and is the only stream that propagates through PowerShell piplines (unless other streams are redirected to success).



## Redirecting
Any of the streams with a numeric identifier can be redirected with the `>` operator.

Use `*` to represent all streams.

Note that the stream identifier must be directly in front of the operator, without spaces: e.g. `6>`.

Gotcha: remote command stream output other than Success or Error does not behave as expected with redirection in PS5.1. Warning, Verbose, and Information will be displayed locally if set to display in the remote command.

### Redirecting to Success
Redirect a stream's output to the success stream by specifying the stream id and redirecting to `&1`, e.g. `6>&1`. Note that success is the only stream that can have other streams redirected to it.

Note that `&1` must be directly after the redirection operator, without spaces.

### Redirecting to Files
Redirect a stream's output to a file by putting the filepath on the right side of the redirection operator, e.g. `6>'C:\temp\info.txt'`.

Use `>>` to append to the file instead of overwriting it.

### Redirecting to `$Null`
Put `$Null` on the right-hand side of the redirection operator to discard the stream's output, e.g. `6>$Null`.

Note that this acts as redirecting to a nameless file, not an example of redirecting to a variable (which is not directly allowed; see Stream Capture Variables).

### Methods and Stream Output
OO methods from PowerShell classes (and possibly others) can only write to output via their `return` statement (`Write-Output` statements are ignored). If they include a `return` statement, attempts to write to stderr are ignored. This appears to be intended to encourage a `return` or `throw` convention.

Other `Write-` methods for stream output appear to be unaffected.



## Stream Capture Variables
"Advanced functions" (any written with `[CmdletBinding()]` or any `[Parameter]`-described parameter) support standard parameters that allow you to capture output to a variable in addition to writing it to the corresponding stream:

* `-OutVariable`
* `-ErrorVariable`
* `-WarningVariable`
* `-InformationVariable`

Always specify a string as the name of the variable to capture to (specifying a variable will attempt capture to the string value of the variable); the variable with corresponding name will be updated within caller's scope to contain a list of the output messages.

Streamed output will also be written to the corresponding stream, unless the stream is otherwise redirected.

Streams redirected and captured at a lower level will still get captured in higher-level capture variables (unsure about specifics).

**Method stream output** will not be captured in the variable, even if the method is used from within non-OO scripting. Method streams are subject to redirection.
