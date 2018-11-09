# Scripting with PowerShell
Similar to other scripting languages, the primary functionality of a PowerShell script is a command, or series of commands. However, there are some definite differences.

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
A script/function can reference the arguments passed to it via the $`Args` array. Use bracket notation (e.g. `$Args[0]`) to reference a single argument.

### Named Parameters
A script/function can define a named set of parameters. Define named parameters that your script/function can accept by name by using a param statement before any non-comment content. Parameter definitions must be comma-separated, and ignore whitespace:

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

## Outputting Data
Any data item that appears on a line on its own is considered part of the output data. E.g. lines

```
"A"
1
```

Will result in output: `"A" 1`.

To write user output data, use one of the `Write` cmdlets:

* `Write-Host` writes general-purpose console output, and includes `foregroundcolor` and `backgroundcolor` parameters to allow styling. This is the primary output channel prior to PowerShell 3.0.
* `Write-Debug` writes debug-level data, which will only be displayed if the `-Debug` (equiv. `-Debug $True`), or the `$debugPreference` pref variable is set to something other than its default `"SilentlyContinue"`, e.g. via `$debugPreference="Continue"`
* `Write-Info` writes info-level data, which will only be displayed if the `-InformationAction` parameter is set to a non-default value (e.g. `Continue`), or the `$informationPreference` variable is set to a non-default value.
* `Write-Warning` writes warning-level data, which will be displayed unless non-default parameter or preference is set.
* `Write-Error` writes error-level data, which will be displayed along with trace data, unless non-default parameter or preference is set

### Outputting Variables
To output a variable as a string (e.g. via write-method), direct references can be included within the quotes. E.g. `"Value of X is $x"` will output the value of x in place of $x. However, to reference the properties of a variable, the variable reference statement must be enclosed in `$()`, e.g. `"Value of obj.x is $($obj.x)"`.