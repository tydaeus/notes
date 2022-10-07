# PowerShell Common Parameters
The PowerShell common parameters can be passed to any advanced function (a function with the `[CmdletBinding()]` attribute or a parameter with the `[Parameter]` attribute); the PowerShell runtime takes care of their implementation. Developer awareness, howver is required to successfully utilize some of them.

Official documentation is at https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_commonparameters?view=powershell-5.1; here I attempt to summarize and list common gotchas.

## `ErrorVariable`, `InformationVariable`, `OutVariable`
These parameters allow specifying a variable that will then be populated with output from the named stream; e.g. to capture the error output from `Invoke-MyFunction` as `$errVar`:

``` PowerShell
Invoke-MyFunction -ErrorVariable 'errVar'
```

Prefix the variable name with `+` to append to the variable's pre-existing content.

Gotchas:
* you must specify the (string) name of the variable, not the variable itself
* the variable will be populated with an arraylist of the objects written; only some operations will coerce this as you may expect (in particular, you can't just throw the error variable on its own)
* this occurs separate from stream output and redirection
* output redirected within the called function instead of output by it will still end up in the output variable



