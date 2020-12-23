# PowerShell `ShouldProcess()`
Condensed from https://adamtheautomator.com/powershell-whatif/. See also https://docs.microsoft.com/en-us/powershell/scripting/learn/deep-dives/everything-about-shouldprocess.

The PowerShell `SupportsShouldProcess` standard function behavior provides standardization and some support for simulated runs, step-by-step confirmation, and enhanced verbosity

## Indicating `ShouldProcess` Support

The cmdlet/function must specify the `CmdletBinding(SupportsShouldProcess)` attribute at its start to indicate it supports `-WhatIf` and `-Confirm`. This designation adds the `-WhatIf` and `-Confirm` standard parameters to its syntax.


## Using `$PSCmdlet.ShouldProcess()`
Check whether actual functionality should be performed using one of the following invocations:

* `$PSCmdlet.ShouldProcess($target)`
* `$PSCmdlet.ShouldProcess($target, $operation)`
* `$PSCmdlet.ShouldProcess($message, $target, $operation)`

`$target` should indicate what item is being targeted by the function (generally a file).

`$operation` defaults to the name of the function or script.

`$message` will replace the default message formatting with a custom string. Only the `-Confirm` message will use the `$target` and `$operation` params when `$message` has been specified; `-Confirm` appears to disregard `$message` and display a sparse message.

### `ShouldProcess()` behavior with `-WhatIf`
If the `-WhatIf` switch has been set, `ShouldProcess()` will return `$False`, and it will output with format `What if: Performing the operation "$operation" on target "$target"`. If the `-WhatIf` switch has not been set, it will return `$True`.

### `ShouldProcess()` behavior with `-Confirm`
If the `-Confirm` switch has been set, `ShouldProcess()` will pause the script and ask the user for confirmation with format:

```
Confirm
Are you sure you want to perform this action?
Performing the operation "$operation" on target "$target".
[Y] Yes [A] Yes to All [N] No [L] No to All [S] Suspend [?] Help (default is "Yes"):
```

`ShouldProcess()` will return `$True` if the operation was confirmed, or `$False` if it was not.

### `ShouldProcess()` behavior with `-Verbose`
If the `-Verbose` switch has been set, `ShouldProcess()` will print verbose output if neither `-WhatIf` nor `-Confirm` is specified, with default format `VERBOSE: Performing the operation "$operation" on target "$target".`




## Inheriting `-WhatIf` Behavior
Commands are supposed to inherit `-WhatIf` behavior, but PowerShell doesn't apply this reliably. So, downstream commands should get invoked with parameter `-WhatIf:$WhatIfPreference` to ensure that the currently configured `-WhatIf` behavior propagates. 




## Inheriting `-Confirm` Behavior
Some commands successfully inherit `-Confirm` behavior, but it's likely you'll want to suppress their confirmations in cases where you've already prompted for confirmation. Specify `-Confirm:$False` to override.