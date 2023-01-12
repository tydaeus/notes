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

`$message` will replace the default message output with a custom string.
* `-WhatIf` processing disregards the `$target` and `$operation` when `$message` is specified, displaying only the contents of `$message`
* `-Confirm` displays the `$target` and `$operation` params on separate lines when `$message` has been specified, appearing to discard the contents of `$message` while also discarding the standard confirm verbiage

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




## Inheriting `ShouldProcess()` Behavior
Commands are supposed to inherit `-WhatIf` and `-Confirm` behavior, but PowerShell's standard commands don't appear to apply this consistently or sensibly.

In many cases you will need to manually specify whether to inherit or override these behaviors. To do so, set the corresponding switch for the downstream command.
* Set to the corresponding preference variable to inherit, e.g.:
    - `-WhatIf:$WhatIfPreference`
    - `-Confirm:$ConfirmPreference`
* Set to desired value to override, e.g.:
    - `-WhatIf:$False`
    - `-Confirm:$False`

Common inheritors to override:
* `Set-Variable`
* `ForEach-Object` when retrieving data or making non-persistent changes

