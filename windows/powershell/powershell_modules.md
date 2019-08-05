# PowerShell Modules
PowerShell modules contain functions that are intended for reuse by other functions/scripts.

## How to Import
Use the `Import-Module` cmdlet to import a PowerShell module.

```PowerShell
# Import module myModule from an explicit location:
Import-Module "C:\myModules\myModule"

# Import module myModule from $env:PSModulePath/myModule/:
Import-Module myModule

# Add a location to $env:PSModulePath to simplify imports:
if (-not $env:PSModulePath.Contains('C:\myModules')) {
    $env:PSModulePath += ';C:\myModules'
}
```

See https://docs.microsoft.com/en-us/powershell/developer/module/importing-a-powershell-module for more information.

### Implicit Imports
In PowerShell 3.0+, any module on a path within `$env:PSModulePath` will be automatically imported if one of its functions is called.



## How to Write a Module
To write a simple module, write a PowerShell script and give it the `.psm1` extension. It will need to be within a folder named after the module if you want to be able to import it without specifying the full path. E.g. `C:\MyModules\myModule\myModule.psm1`.

By default, any functions defined within the script will be available to any script that uses `Import-Module` on it, properties will not be available. Explicitly using `Export-ModuleMember` within the module script allows you to customize what functions, aliases, and variables will be made available when `Import-Module` is called without parameters. `Import-Module` parameters allow you to customize what exported members are imported into your PowerShell session.

```PowerShell
Export-ModuleMember -Function Do-Something
```

See https://docs.microsoft.com/en-us/powershell/developer/module/how-to-write-a-powershell-script-module and https://docs.microsoft.com/en-us/powershell/developer/module/how-to-write-a-powershell-module-manifest for more information.
