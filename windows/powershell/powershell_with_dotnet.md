# Using PowerShell with .Net

## Referencing .Net Types
.Net types can be referenced by placing their fullname within brackets, e.g. `[System.IO.Path]`. "System" can be dropped in cases where this does not lead to ambiguity, e.g. `[IO.Path]`.

### The `using` Statement
In PowerShell 5.0+, the `using` statement is available to allow C#-like easy referencing to namespaces. E.g. `using namespace Long.NameSpace.Example.Name`.


## Adding Types
Everything under the .Net `System` namespace is generally available within PowerShell without any additional effort.

Additional .Net libraries must be requested explicitly using the `Add-Type` cmdlet, or by using .Net libraries to add them (e.g. `System.Reflection.Assembly`).

### Add-Type
By name: `Add-Type -Assembly My.AssemblyName`. This typically works with MS-provided .Net libraries.

By location: `Add-Type -Path C:\path\to\My.AssemblyName.dll`

### `System.Reflection.Assembly`
By name: `[System.Reflection.Assembly]::LoadWithPartialName('My.AssemblyName')`. This typically works well for 3rd-party libraries that have been added to the GAC.

By full path: `[System.Reflection.Assembly]::LoadFile('C:\path\to\My.AssemblyName.dll')`.
