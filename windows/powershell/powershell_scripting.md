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
