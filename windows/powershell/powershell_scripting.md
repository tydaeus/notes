# Scripting with PowerShell
Similar to other scripting languages, the primary functionality of a PowerShell script is a command, or series of commands. However, there are some definite differences.

## Terminology
A PowerShell script is referred to as a "Function", not be confused with the functions defined within it using the `function` keyword. *Sigh*.

A PowerShell utility written by Microsoft or others in a .NET programming language is referred to as a "cmdlet".

###  Advanced Functions
Functions that either have the `[cmdletbinding()]` attribute before the param block (which must be present but can be empty), or that include at least one parameter with the `[parameter]` attribute, are *Advanced*. Advanced functions:

* gain the "common parameters" for use by cmdlets they call
* can modify how their parameters are handled via the `[parameter]` attribute
* get the `$PSCmdlet` variable injected for access to information about themselves
    - `$PSCmdlet.MyInvocation.BoundParameters` is a dictionary that contains the values of the bound parameters; use the `.isPresent` property to check if switch parameters were provided

For example, the `Write-Information` cmdlet by default will perform the `SilentlyContinue` action, displaying nothing. In a non-"Advanced" function, the only way to alter this is via the `$informationPreference` variable. In an "Advanced" function, if the function is called with parameter `-InformationAction Continue`, all `Write-Information` calls within the function will display output unless the parameter is provided specifically for that call.


## Comments
Use `#` for comment lines.

Use `<#` and `#>` for block comments.


## Get-Help Documentation
You should provide documentation viewable through Get-Help as part of your script. This is described in a specially formatted comment, such as:

```
<#
.SYNOPSIS
    A brief description of what this script does.
.DESCRIPTION
    A longer description of what this script does.
.PARAMETER Parameter1
    A description of Parameter1
.EXAMPLE
    Example of using the script
.NOTES
    Miscellaneous notes, such as versioning and authoring.
#>
```

## Arguments
A script/function can reference the arguments passed to it via the `$Args` array. Use bracket notation (e.g. `$Args[0]`) to reference a single argument.

### Named Parameters
A script/function can define a named set of parameters. Define named parameters that your script/function can accept by name by using a param statement before any non-comment content.

A hashtable of named parameters passed into a script/function can be accessed using `$PSBoundParameters`; the `$Args` variable will no longer be populated.

Parameter definitions must be comma-separated, and ignore whitespace:

```
param(
    [PARAM1TYPE]$PARAM1NAME,
    [PARAM2TYPE]$PARAM2NAME
)
```

Parameters can be further qualified by adding parameter attributes. These appear in brackets prior to the parameter type specifier, e.g.:

```
param(
    [parameter(Mandatory=$true)][string]$MyParam,
    [alias("MP2")][string]$MyParam2
)
```

The `Parameter` attribute allows setting multiple arguments, in key-value format.

Useful `Parameter` attribute arguments:

* `Mandatory=$true` - sets this parameter to be mandatory

### Switch Parameters
Switch parameters act as boolean, defaulting to false if the parameter isn't provided and changed to true if it is.

Declare: `[switch] $MySwitch`

Use: `.\My-Script.ps1 -MySwitch`

## Outputting Data
Any data item that appears on a line on its own is considered part of the output data. E.g. lines

```
"A"
1
```

Will result in output: `"A" 1`.

The `Write-Output` command has the same effect.

### Output to Null
Because any output from a line that isn't explicitly passed to a null-output command or stored in a variable is treated as an output from your script, you will probably wish to write your output to null sometimes. To do this, pipe to `Out-Null`.

### User Feedback Output
To write user feedback output data, use one of the `Write` cmdlets:

* `Write-Host` writes general-purpose console output, and includes `foregroundcolor` and `backgroundcolor` parameters to allow styling. This is the primary output channel prior to PowerShell 3.0.
* `Write-Debug` writes debug-level data, which will only be displayed if the `-Debug` (equiv. `-Debug $True`), or the `$debugPreference` pref variable is set to something other than its default `"SilentlyContinue"`, e.g. via `$debugPreference="Continue"`
* `Write-Information` writes info-level data, which will only be displayed if the `-InformationAction` parameter is set to a non-default value (e.g. `Continue`), or the `$informationPreference` variable is set to a non-default value.
* `Write-Warning` writes warning-level data, which will be displayed unless non-default parameter or preference is set.
* `Write-Error` writes error-level data, which will be displayed along with trace data, unless non-default parameter or preference is set

*Note*: PowerShell is stupid, and `Write-Error` does not write to stderr as visible from outside of PowerShell.

*Note*: See above section on "Advanced" functions for how to allow parameters passed to the script to propagate to these commands.

### Outputting Variables
To output a variable as a string (e.g. via write-method), direct references can be included within the quotes. E.g. `"Value of X is $x"` will output the value of x in place of $x. However, to reference the properties of a variable, the variable reference statement must be enclosed in `$()`, e.g. `"Value of obj.x is $($obj.x)"`.

## Error Checking/Handling

### ErrorActionPreference
Set `$ErrorActionPreference = "Stop"` to automatically exit after the first error encountered. Note that this does not cause non-script commands that resulted in an error to exit.

### `$error`
When errors occur, they are stored in the `$error` variable. This array is populated in LIFO order, so the most recent error is stored in `$error[0]`. The size of this array is limited by `$MaximumErrorCount`, causing it to act like a buffer.

### `$?`
`$?` holds a boolean that indicates whether the last command succeeded. This will be `$True` if the last command succeeded, `$False` if it exited with a non-zero code or due to an uncaught `throw` statement.

Note that this very much the *last command*, so it should be checked immediately if desired. Even evaluating `$LastExitCode` can overwrite `$?`.

### `$LastExitCode`
`$LastExitCode` holds the exit code from the last command. Note that exiting due to an uncaught `throw` statement does not result in a non-zero status.
