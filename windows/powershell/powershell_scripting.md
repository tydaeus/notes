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
You should provide documentation viewable through Get-Help as part of your script. This is described in a specially formatted comment at the top of the script (or before each function), such as:

``` PowerShell
<#
.SYNOPSIS
    A brief description of what this script does.
.DESCRIPTION
    A longer description of what this script does.
.PARAMETER Parameter1
    A description of Parameter1
.EXAMPLE
    Example of using the script. The first line is assumed to be the commandline invocation, and will be formatted as such when displayed.
.NOTES
    Miscellaneous notes, such as versioning and authoring.
#>
```

## Arguments
A script/function can reference the arguments passed to it via the `$Args` array. Use bracket notation (e.g. `$Args[0]`) to reference a single argument.

### Named Parameters - PSBoundParameters
A script/function can define a named set of parameters. Define named parameters that your script/function can accept by name by using a param statement before any non-comment content.

A hashtable of named parameters passed into a script/function can be accessed using `$PSBoundParameters`; the `$Args` variable will no longer be populated. `$PSBoundParameters.ContainsKey($paramName)` will return a boolean indicating whether parameter `$paramName` was specified. `$PSBoundParameters` captures only *explicitly* specified parameters, not those that retain their default value.

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
    [parameter(Mandatory)][string]$MyParam,
    [alias("MP2")][string]$MyParam2
)
```

The `Parameter` attribute allows setting multiple arguments, in key-value format.

Useful `Parameter` attribute arguments:

* `Mandatory` - sets this parameter to be mandatory
* `Position=0` - sets this parameter to correspond to first invocation argument
* `ValueFromRemainingArguments` - sets this parameter to be populated by all parameter values that aren't assigned to other parameters

Note that boolean `Parameter` attribute arguments typically default to `$True` if specified and `$False` if not specified, so `[Parameter(Mandatory=$True)]` is equivalent to `[Parameter(Mandatory)]`, while `[Parameter(Mandatory=$False)]` is equivalent to not omitting the `Mandatory` argument entirely.

Other useful attributes:

* `[AllowNull()]` - allows a null value to be passed to a parameter
* `[ValidateScript({ ScriptBlock })]` - use passed ScriptBlock to validate the parameter
* `[AllowEmptyString()]` - allows passing an empty string to a string parameter

### Splatting Parameters
Prefix a passed HashTable or Array variable with `@` instead of `$` to "splat" the contained values into multiple parameters.

``` PowerShell
# Array form specifies parameters by positional order:
$params = @('a', 'b', 'c')
# invoke Do-TheThing equivalent to Do-TheThing 'a' 'b' 'c'
Do-TheThing @params

# HashTable form explicitly names the parameters
$params = @{
    param1 = 'a'
    param2 = 'b'
    param3 = 'c'
}
# invoke Do-TheThing equivalent to Do-TheThing -param1 'a' -param2 'b' -param3 'c'
Do-TheThing @params
```

Splat `Args` (`@Args`) or `PSBoundParameters` (`@PSBoundParameters`) to forward all passed parameters to the invoked function.

Note that only Arrays and HashTables can be splatted. Any other object must be converted to an Array or HashTable before it can be splatted; for a PsCustomObject the following function can be used:

``` PowerShell
function Convert-PsCustomObjectToHashTable {
    param (
        [Parameter(Mandatory=$True, ValueFromPipeline=$True)]
        [PsCustomObject]
        $obj
    )

    Process {
        $result = @{}
        $obj.psobject.properties | % {
            $result[$_.Name] = $_.Value
        }

        return $result
    }
}
```

See https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_splatting?view=powershell-6 for full information.

#### Converting All Parameters to Splatted Parameters
If you desire to capture all of a function/script's input parameters for purposes of passing them along to another function/script, you'll need to iterate through them in a manner similar to the following:

```PowerShell
$parameters = @{}

foreach ($key in $MyInvocation.MyCommand.Parameters.Keys) {
    $value = Get-Item "Variable:$key" -ErrorAction 'Ignore'

    if ($value) {
        $outParameters[$key] = $value.Value
    }
}
```


### Parameter Typing and Coercion
PowerShell will attempt to coerce explicitly typed parameters into appropriate type if they aren't passed as such. This results in interesting behaviors with nulls - a null string will be converted to empty, a null int will be converted to 0, etc.

### Multiple Parameter Sets
Sometimes a script function will need to support different combinations of parameters. For this reason, the `parameter` attribute also supports the `ParameterSetName` property.

```PowerShell
param(
    # if this parameter is detected, we're using ParamSetA
    [parameter(Mandatory=$True, ParameterSetName="ParamSetA")]
    [string]
    $Param1A,

    # if this parameter is detected, we're using ParamSetB
    [parameter(Mandatory=$True, ParameterSetName="ParamSetB")]
    [string]
    $Param1B,

    # this parameter can be provided as part of either ParamSetA or ParamSetB
    [parameter(Mandatory=$True, ParameterSetName="ParamSetA")]
    [parameter(Mandatory=$True, ParameterSetName="ParamSetB")]
    [string]
    $Param2AB
)
```

Note that all parameters must be in at least one parameter set.

Specify which parameter set is used if multiple sets are applicable by specifying the `DefaultParameterSetName` cmdlet property in the CmdletBinding statement, e.g. `[CmdletBinding(DefaultParameterSetName='MyDefaultParameterSet')]`. Note that if no parameters are part of this set, this set will be in use when no parameters are passed.

Check which parameter set was used via the `$PsCmdlet.ParameterSetName` variable.

### ValidateSet
Use the `ValidateSet` parameter attribute to restrict potential values of a parameter:

```PowerShell
[ValidateSet("Val1", "Val2", "Val3")][string]$myParam
```

### Switch Parameters
Switch parameters act as boolean, defaulting to false if the parameter isn't provided and changed to true if it is.

Declare: `[switch] $MySwitch`

Use: `.\My-Script.ps1 -MySwitch`



### Pipeline Input Parameters

#### `ValueFromPipeline`
If a parameter has the `ValueFromPipeline` parameter attribute set to true, its value will be populated with a series of objects from pipeline intput. Use a ForEach loop to process the piped input.

This will be processed via the `Begin`, `Process`, and `End` blocks discussed below.

```PowerShell
param(
    [parameter(Mandatory = $True, ValueFromPipeline = $True)]
    $pipedInput
)
```

#### `ValueFromPipelineByPropertyName`
If a parameter has the `ValueFromPipelineByPropertyName` parameter attribute set to true, its value will be populated with a series of property values stripped from the pipeline input objects based on their names; e.g. if the parameter's name is `$Name`, the series of `$Name` property values will be piped in.

This series of properties will be processed via the `Begin`, `Process`, and `End` blocks discussed below.

```PowerShell
param(
    [parameter(Mandatory = $True, ValueFromPipelineByPropertyName = $True)]
    [string[]]
    $Name
)
```

#### `Begin`, `Process`, and `End`
To process pipeline input, `Begin`, `Process`, and `End` blocks should be populated to define behavior.

**WARNING**: if  `Process` block is specified, any code specified outside `Begin`, `Process`, or `End` will result in error "Cannot evaluate parameter 'Name' because its argument is specified as a script block and there is no input. A script block cannot be evaluated without input."

Once defined, the `Begin`, `Process`, and/or `End` blocks will be used to define your function's execution regardless of whether it has been called with pipeline input or not.

```PowerShell
Begin {
    # this block runs once at the start, use for variable init, etc.
}

Process {
    # do stuff on the input
    # Process will run once for each item in the pipeline.
    # Items can be referenced as $PSItem (aka $_) or as the pipeline parameter
}

End {
    # this block runs once at the end, use for cleanup etc.
}
```



## Outputting Data
Any data item that appears on a line on its own is considered part of the output data. E.g. lines

```
"A"
1
```

Will result in output: `"A" 1`.

The `Write-Output` command has the same effect.

### The `return` Command
Using the `return` command will both output the specified value and act as an end of execution for the current function or script.

### Output to Null
Because any output from a line that isn't explicitly passed to a null-output command or stored in a variable is treated as an output from your script, you will probably wish to write your output to null sometimes. To do this, pipe to `Out-Null`.

### User Feedback Output
To write user feedback output data, use one of the `Write` cmdlets:

* `Write-Host` writes general-purpose console output, and includes `foregroundcolor` and `backgroundcolor` parameters to allow styling. 
    - Historical/Technical Notes:
        + This is the primary user feedback output channel prior to PowerShell 3.0
        + In PowerShell 5.0+, this is treated as a wrapper for `Write-Information` that always produces output unless `-InformationAction` is set to `Ignore` (or the channel is redirected to `$Null`).
        + Opinions abound online about `Write-Host` being harmful due to its unredirectable behavior between 3.0 and 5.0, but its 5.0+ wrapper behavior addresses this
* `Write-Debug` writes debug-level data, which will only be displayed if the `-Debug` (equiv. `-Debug $True`), or the `$debugPreference` pref variable is set to something other than its default `"SilentlyContinue"`, e.g. via `$debugPreference="Continue"`
* `Write-Information` writes info-level data, which will only be displayed if the `-InformationAction` parameter is set to a non-default value (e.g. `Continue`), or the `$informationPreference` variable is set to a non-default value.
* `Write-Warning` writes warning-level data, which will be displayed unless non-default parameter or preference is set.
* `Write-Error` writes error-level data, which will be displayed along with trace data, unless non-default parameter or preference is set

*Note*: PowerShell is stupid, and `Write-Error` does not write to stderr as visible from outside of PowerShell.

*Note*: See above section on "Advanced" functions for how to allow parameters passed to the script to propagate to these commands.

### String Interpolation
To include a variable within a string for output (e.g. via write-method), direct references can be included within a double-quoted string. E.g. `"Value of X is $x"` will output the value of x in place of $x. To access the properties of a variable or use any operators other than the `:` operator, the variable reference statement must be enclosed in `$()`, e.g. `"Value of obj.x is $($obj.x)"`; this allows insertion of any statement's string output.



## ScriptBlocks
Script blocks allow you to encapsulate short scripting statements for passing as a parameter.

### Defining ScriptBlocks
Define a ScriptBlock by enclosing a list of statements in braces: `{<statement list>}`.

If the ScriptBlock needs to accept parameters, these must be defined by using a `param()` statement as the first statement:

```PowerShell
{
    param($a, $b)
    return $a + $b
}
```

### Invoking ScriptBlocks
Invoke a ScriptBlock by using the `Invoke-Command` cmdlet: `Invoke-Command -ScriptBlock $ScriptBlock -ArgumentList $Param1, $Param2...`.

More simply, use the `call` operator: `&$ScriptBlock $Param1 $Param2`.

### Delay-Bind ScriptBlocks
A PowerShell function that defines a **typed pipeline (by value or by PropertyName)** parameter that is not of type `[scriptblock]` or `[object]` can implicitly accept delay-bind scriptblock parameters which can thereby utilize the `$PSItem`/`$_` automatic variable. E.g.:

``` PowerShell
function Test-DelayedBinding {
    param(
        # this is our typed pipeline parameter
        [Parameter(ValueFromPipeline, Mandatory)][string]$string,
        # this is our scriptblock parameter
        [Parameter(Position=0)][scriptblock]$sb
    )

    Process {
        if (&$sb $string) {
            Write-Output $string
        }
    }
}

>'foo', 'fi', 'foofoo', 'fib' | Test-DelayedBinding { return $_ -match 'foo' }
'foo'
'foofoo'
```

The delay-bind scriptblock must used named parameters if any other parameters are desired; i.e. it cannot use `$args`.

Only delay-bind scriptblocks can utilize the `$PSItem`/`$_` automatic variable.

Building these custom delay-bind invocation functions can be a little tricky, and they don't appear to have the scope inheritance applied to the built-in delay-bind invocation functions. **Where possible, use the built-in** delay-bind functions, e.g. `ForEach-Object` for transformations and `Where-Object` for filtering.

### Converting a Function to a ScriptBlock
Access an in-scope function's definition (without its name) via `${function:Function-Name}`. This definition can then be passed in lieu of a ScriptBlock.

`[ScriptBlock]::Create()` can be used to create a ScriptBlock from an arbitrary string. You can use this in combination with function definitions to pass multiple function definitions into other scripts and then dot-source them into that scope, potentially allowing them to call each other, e.g. for using Invoke-Command on a remote computer.

```PowerShell
$functionDefinitions = @"
    function Do-Something { ${function:Do-Something} };
    function Do-SomethingElse { ${function:Do-SomethingElse} }
"@

Invoke-Command -ComputerName OTHERCOMPUTER -ScriptBlock {
    param(
        $functionDefinitions
    )

    . ([ScriptBlock]::Create($functionDefinitions))

    Do-Something | Do-SomethingElse
} -ArgumentList $functionDefinitions

```

Use the `function` PSDrive if you need to access a function whose name is in a variable, e.g. `Get-Content "Function:$functionName"` will retrieve the definition of the function whose name is stored in `$functionName`.

This can be used to package a function for re-declaration in another scope, via:

```PowerShell
<#
.SYNOPSIS
    Returns a string containing declaration and definition for a named function(s).
#>
function Get-PackagedFunction {
    param([Parameter(Mandatory=$True, ValueFromPipeline=$True)][string]$FunctionName)

    process {
        if (Test-Path "function:$FunctionName") {
            return "function $FunctionName { $(Get-Content "function:$FunctionName") }"
        } else {
            throw { "No function named '$FunctionName' within scope."}
        }    
    }
}
```


## String Declaration and Interpolation
Strings can be declared using either single or double-quotes; this controls their behavior.

### Single-Quotes - Literal Strings
Strings declared with single-quotes will not undergo any variable interpolation: `$myString = 'The name of the variable is $myVar'`

### Double-Quotes - Interpolated Strings
Strings declared with double-quotes will interpolate any variables within them: `$myString = "Value of myVar: $myVar"`.

Use `$()` to interpolate an expression, such as referencing a variable property or function: `$myString = "myVar.Value: $($myVar.Value)"`.

Use `${}` to escape a variable name to resolve any ambiguity: `$myString = "Prefix${Value}Suffix"`

### Multiline Strings - "Here-string"
You can declare a multi-line string by wrapping it in either `@""@` or `@''@`; interpolation will be performed if enclosed with `@""@`. The opening characters must be the last characters on their line, and the closing characters must be the first characters on their line. Note that quotation marks (`"`) can be used without escaping inside either form.

``` PowerShell
$str = @"
Some "Text"
More text.
@"
```
