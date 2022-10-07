# PowerShell Statements

## `if` Statement

``` PowerShell
if (<condition>) {
    [actions]
} elseif (<condition>) {
    [actions]
} else {
    [actions]
}
```

Condition statements are coerced to booleans for evaluation purposes.

## `switch` Statement

``` PowerShell
switch [-regex | -wildcard | -exact] [-casesensitive] <(<test-expression>)| -file <filepath>>
{
    < "string" | number | variable | { <value-scriptblock> } > { <action-scriptblock> }
    ...
    [ default { <action-scriptblock> } ]
}
```

`$_` can be used in the matching statement to reference the result of the test expression.

Each condition statement is tested and its action is run if it matches (not using the fallthrough behavior of C-family switches). Use `continue` to stop processing the current item and proceed to the next (if operating on a collection), or `break` to stop the switch entirely.

Official doc at: https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_switch