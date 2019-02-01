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
By name: `[System.Reflection.Assembly]::LoadWithPartialName('My.AssemblyName')`. This typically works well for 3rd-party libraries that have been added to the GAC. Note that this method is deprecated, but I am unaware of a similarly succinct replacement.

By full path: `[System.Reflection.Assembly]::LoadFile('C:\path\to\My.AssemblyName.dll')`.

## 64-bit v. 32-bit
If you attempt to run a 32-bit or partially 32-bit dll from a 64-bit PowerShell, you're going to have a bad time.

### Check if 64-bit
Check if PowerShell is currently running in 64-bit mode via `[Environment]::Is64BitProcess`; this is a boolean that will be true if running as 64-bit.

### Executing 32-bit PowerShell
The 32-bit version of PowerShell gets installed as `C:\Windows\SysWOW64\WindowsPowerShell\v1.0\powershell.exe`. Note that this *is* the 32-bit version, despite being installed in SysWOW64, and *is not* necessary PowerShell 1.0, despite being installed in a v1.0 folder.

### Starting a 32-bit Remote Session
Use `Enter-PSSession -ComputerName $ComputerName -ConfigurationName Microsoft.PowerShell32` to start a 32-bit remote session on a 64-bit host. Note that this configuration name does not exist on 32-bit hosts.
