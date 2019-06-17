# PowerShell Modules
PowerShell modules contain functions that are intended for reuse by other functions/scripts.

## How to Import
Use the `Import-Module` cmdlet to import a PowerShell module.

```PowerShell
# Import module myModule from an explicit location:
Import-Module "C:\myModules\myModule"

# Import module myModule from a location contained within the $env:PSModulePath:
Import-Module myModule

# Add a location to $env:PSModulePath to simplify imports:
$env:PSModulePath = $env:PSModulePath + ';C:\myModules'
```

See https://docs.microsoft.com/en-us/powershell/developer/module/importing-a-powershell-module for more information.

## How to Write a Module
To write a simple module, just write a PowerShell script and give it the `.psm1` extension. Any functions defined within the script will be available to any script that uses `Import-Module` on it, properties will not be available.

See https://docs.microsoft.com/en-us/powershell/developer/module/how-to-write-a-powershell-script-module and https://docs.microsoft.com/en-us/powershell/developer/module/how-to-write-a-powershell-module-manifest for more information.
