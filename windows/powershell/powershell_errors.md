# PowerShell Errors

## Error Checking/Handling

### ErrorActionPreference
Set `$ErrorActionPreference = "Stop"` to automatically exit after the first error encountered. Note that this does not cause non-script commands that resulted in an error to exit.

### `$Error`
When errors occur, they are stored in the `$Error` variable. This array is populated in LIFO order, so the most recent error is stored in `$Error[0]`. The size of this array is limited by `$MaximumErrorCount`, causing it to act like a buffer.

Useful fields:

* `Exception` - stores a brief description of the error
* `ScriptStackTrace` - Provides a stack trace to where the error occurred

```PowerShell
# assuming a function named log
$log "An error occurred: $($Error[0].Exception)"
$log "$($Error[0].ScriptStackTrace)"
```

### `$?`
`$?` holds a boolean that indicates whether the last command succeeded. This will be `$True` if the last command succeeded, `$False` if it exited with a non-zero code or due to an uncaught `throw` statement.

Note that this very much the *last command*, so it should be checked immediately if desired. Even evaluating `$LastExitCode` can overwrite `$?`.

### `$LastExitCode`
`$LastExitCode` holds the exit code from the last command. Note that exiting due to an uncaught `throw` statement does not result in a non-zero status. This variable does not automatically get set back to zero, so it may be necessary to do `$global:LastExitCode = 0` prior to running a command to ensure that it does not 

### Try/Catch/Finally
PowerShell supports `try`, `catch`, and `finally` blocks similar to other languages. However, there are some interesting quirks.

Only PowerShell scripts/cmdlets that exit by an exception bubbling will result in `catch` processing.

Scripts/programs that exit with an error status instead of by throwing an exception will not be caught in the `catch` block, but the `finally` block will be run.

Within the `catch` block, the exception can be accessed from the `$PSItem`/`$_` variable.
