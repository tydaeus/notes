# PowerShell Errors

## Error Checking/Handling

### `$ErrorActionPreference` Config Variable
Set the `$ErrorActionPreference` variable to control how PowerShell handles errors.

* `Continue` (Default) - write the error to the error stream and continue running
* `Inquire` - output error message and ask user whether to continue
* `SilentlyContinue` - don't output message, continue execution
* `Stop` (My general preference for scripts) - automatically exit after the first error

Fatal errors and uncaught exceptions will result in exit regardless of preference. Output to `stderr` and non-zero exit statuses are treated as errors.

Be sure to test how the commands in question report their status - error/non-error status varies widely by implementation, even within Microsoft's standard commands and cmdlets.

`$ErrorActionPreference` is subject to standard scoping rules - functions/scriptblocks inherit its starting value from their parent environment, and changes made to `$ErrorActionPreference` within a function/scriptblock without a scope qualifier will not modify parent environment.

### `ErrorAction` Parameter
The `ErrorAction` standard parameter can be used to specify how errors should be handled while a function is executing.

The `ErrorAction` parameter supports the following values in addition to those supported by `$ErrorActionPreference`:

* `Ignore` - suppress error message and continue executing the command, *without adding the error to `$Error`*
* `Suspend` - on error, suspend command for investigation, allowing resumption

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



## Error Reporting
There are multiple options for reporting errors from within a PowerShell script or function.

### Throw
Throwing an exception via the `throw` keyword is probably the best general-purpose option, because it allows the caller to catch it with a `catch` block or not.

``` PowerShell
throw "Something bad happened"
```

However, if the exception does bubble up to the command line, it will include detailed trouble-shooting information that may be undesirable.

### Exit
The `exit` keyword can be used to cause a script to exit prematurely; specifying a non-zero exit code indicates that this was due to an error.

This should only be used within a script context (rather than a reusable function context) because it will exit the execution environment. Thus if a reusable function invokes `exit`, it will end the calling script or the PowerShell session that called it.