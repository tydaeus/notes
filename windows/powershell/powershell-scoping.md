# PowerShell - Scoping
PowerShell does a lot to maintain discrete scopes, some of it intuitive, some of it not.

## Environment Scope
Environment variables can only be accessed with the `env:` scope modifier.

## Accessing Caller's Scope
An advanced function can access its caller's scope, even if the function is defined within a module, by retrieving it from the `$PSCmdlet` variable:

``` PowerShell
function Get-CallerVar {
    # need to include CmdletBinding or at least one parameter with a Parameter attribute to make the function advanced
    [CmdletBinding()]

    param(
        [string $VarName] # name of the caller variable to retrieve
    )

    return $PSCmdlet.SessionState.PSVariable.Get($VarName).Value
    # alternatives:
    #   - $PSCmdlet.GetVariableValue($VarName)
    #   - $PSCmdlet.SessionState.PSVariable.GetValue($VarName)
}
```

### Using Caller's Scope to Implement Custom Preference Variables
By reading a caller's scope for specific variables and using them as the defaults for parameters, we can create custom preference variables that operate similar to PowerShell's standard preference variables.

``` PowerShell
function Test-CustomPreferenceVariables {
    [CmdletBinding()]
    param(
        [string]
        $Param1,

        [string]
        $Param2
    )

    # use a HashTable to map the correspondence between preference variables and parameter names
    # this can be moved outside the function in most cases
    $PARAM_TO_PREF = @{
        Param1 = 'TestParam1Preference'
        Param2 = 'TestParam2Preference'
    }

    # apply preference variables
    foreach ($param in $PARAM_TO_PREF.Keys) {
        $pref = $PARAM_TO_PREF[$param]

        if ((-not $PSBoundParameters.ContainsKey($param)) -and 
            ($prefValue = $PSCmdlet.SessionState.PSVariable.Get($pref).Value)) # needs modification if supporting falsey values
        {
            Set-Variable -Name $param -Value $prefValue
        }
    }

    #...
}

```



## Scoping and the `-Scope` Parameter
Several cmdlets, including `Get-Var`, `Set-Var`, and `Remove-Var` include a `-Scope` parameter that allows you to operate on a specific scope.

This can take either a string value as one of 'Global', 'Local', 'Script', 'Private', or a numeric value where 0 is the current scope and each +1 is another parent level. There doesn't appear to be a way to detect how many parent levels are available (except possibly by grabbing the stack). The parent levels do not apply inheritance the way the PSCmdlet SessionState PSVariable does.
