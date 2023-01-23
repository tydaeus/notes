# PowerShell Modules
PowerShell modules contain functions that are intended for reuse by other functions/scripts.

## How to Import
Use the `Import-Module` cmdlet to import a PowerShell module.

```PowerShell
# Import module myModule from an explicit location:
Import-Module "C:\myModules\myModule"
# ensure that there is only `myModule.psm1` or `myModule.psd1` at this location

# Import module myModule from $env:PSModulePath/myModule/:
Import-Module myModule

# Add a location to $env:PSModulePath to simplify imports:
if (-not $env:PSModulePath.Contains('C:\myModules')) {
    $env:PSModulePath += ';C:\myModules'
}
```

See https://docs.microsoft.com/en-us/powershell/developer/module/importing-a-powershell-module for more information.

Modules are imported within global scope, instead of restricted to use within the importing script. Conflicting identifiers result in excessive fun as a result.

Module statements are only run at the time of the first import. Subsequent import statements do not have any effect on the environment unless a `Remove-Module` is performed.

### Implicit Imports
In PowerShell 3.0+, any module on a path within `$env:PSModulePath` will be automatically imported if one of its functions is called.



## How to Write a Module
To write a simple module, write a PowerShell script and give it the `.psm1` extension. It will need to be within a folder named after the module if you want to be able to import it without specifying the full path. E.g. `C:\MyModules\myModule\myModule.psm1`.

By default, any functions defined within the script will be available to any script that uses `Import-Module` on it, properties will not be available. Explicitly using `Export-ModuleMember` within the module script allows you to customize what functions, aliases, and variables will be made available when `Import-Module` is called without parameters. `Import-Module` parameters allow you to customize what exported members are imported into your PowerShell session.

```PowerShell
Export-ModuleMember -Function Do-Something
```

See https://learn.microsoft.com/en-us/powershell/scripting/developer/module/how-to-write-a-powershell-script-module and https://learn.microsoft.com/en-us/powershell/scripting/developer/module/how-to-write-a-powershell-module-manifest for more information.

### With a Module Manifest
More complex modules should include a module manifest. The manifest is a .psd1 document that declares metadata and setup configuration for the module.

The manifest's name is the module name and must match the name of the containing folder (e.g. `MyModule\MyModule.psd1`). Do not include a .psm1 file with the same name within the subdirectory, because `Import-Module MyModule` will result in that file being imported directly as well and shadowing the manifest.

#### Creating a Module Manifest
1. Use `New-ModuleManifest -Path $MyModulePath` to create the initial template (ensure the filename ends in `.psd1`)
2. Modify the fields in the generated HashTable to describe the module. In particular:
    - RootModule - this should be the `.psm1` (or binary) module file; cannot share name with manifest if in same dir
    - ScriptsToProcess - list of scripts to run before importing the module
        + this should be any one-time setup
        + runs before any of the required modules are run
        + operates within global scope, so functions and variables defined in these scripts end up in the global scope
    - RequiredModules - list of modules to import into global scope before importing this module
        + these must be available on `$env:PSModulePath`, cannot be referenced by file path
    - FunctionsToExport - list of module-defined functions to make available in caller's scope
    - VariablesToExport - list of module-defined variables to make available in caller's scope
    - AliasesToExport - list of module-defined aliases to make available in caller's scope
3. `Test-ModuleManifest $ManifestPath` can be used to verify that the paths referenced are accurate; for full testing you will need to import it and try it out


## Variable Scoping
Variables declared within the module by default effectively act as being within their own module-specific scope. Functions defined in the module access and operate on these variables by default. These variables cannot be accesed from outside the module's scripting and function.

If a variable is exported from the module by the `Export-ModuleMember` cmdlet, it becomes part of the `global` scope, allowing scripts to read and overwrite it (at which point internal functions will also reference the global version).

## In-Memory Module Creation
Use `New-Module` to create a module in-memory (without needing a definition file); this takes a ScriptBlock as a parameter to perform the module definitions.

``` PowerShell
New-Module {
    function MyFunction {
        #...
    }
}
```