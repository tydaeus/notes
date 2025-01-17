# Getting Started with Powershell
Notes on first-time powershell. 

## Starting Powershell
There are multiple ways to start an interactive PowerShell CLI:
* run `powershell.exe`
* from `cmd`, run `powershell` (use `start powershell` instead for a new window)
* run `powershell_ise.exe` - this is the PowerShell Interactive Scripting Environment, which was an early attempt by Microsoft to provide a PowerShell development environment. This was never particularly well optimized, and Microsoft has dropped support for it (but it remains pre-installed in many Windows environments).
* Visual Studio Code provides an embedded PowerShell terminal client. This does provide some developer-type PowerShell features (as does the VSCode text editor), but it is good to remember that it runs with VSCode-specific configurations and plugins. I have also observed bugs occurring specific to this terminal, possibly as a result of conflicts with other features of the environment I was running it in.
* Windows Terminal (`wt`) allows creating tabbed PowerShell sessions. This does not support dragging files into the terminal to enter their paths, unlike other terminals, and may have other irregularities.

## Running Scripts
Scripts are conventionally stored in `.ps1` files. Run the script by:
* right-clicking and selecting `Run with PowerShell`
* from within PowerShell, type the name of the script including relative path, e.g. `.\MyScript.ps1`; anything after the script name is taken as arguments for the script invocation
    - if there are spaces in the path, you will need to pass the path as a string and use the `&` invocation operator, e.g. `&'.\my spaced folder\MyScript.ps1'`
    - use the `.` operator to run the script within the current scope (aka "dot-sourcing") instead of within a child scope, e.g. `. .\MyScript.ps1`; this will result in defined functions and/or variables
* from `cmd` run `PowerShell <scriptpath>`; any further text after the script name is taken as an argument to the script

`PowerShell.exe` can also be run from within a PowerShell session. `PowerShell -File <scriptpath>` will invoke the script to operate within the current environment, similar to PowerShell "dot-sourcing". `PowerShell -Command <scriptpath>` will invoke it within a child scope.

## Setting ExecutionPolicy
Use `Get-ExecutionPolicy` to check what the current execution policy is. This should get set to RemoteSigned via `Set-ExecutionPolicy RemoteSigned` if it is not already set to RemoteSigned.

## About Cmdlets
Cmdlets are the powershell command functions. About cmdlets:

* system, user, and custom cmdlets are available
* output as an object or array of objects
* can retrieve data or pipe it
* names are case-insensitive
* named in verb-noun format, where the verb defines what is done and the noun defines what it's being done to
    - use `Get-Verb` for a list of legal verbs
* use `Get-Help -Category cmdlet` to list all cmdlets
* can accept parameters in `-ParameterName ParameterValue` format; e.g. `Get-Service -Name W*`
    - positional parameters can omit the parameter name; these have the name listed in `[]` in the help syntax
    - parameter names can be truncated, as long as they remain unambiguous
* may have an alias; use `Get-Alias` to list all aliases
    - use `Get-Alias -Name aliasName` to get the true name of the alias named "aliasName"

## Getting Help
* use `Get-Help -Category cmdlet` to list all cmdlets
    - use the `-Full` parameter to get more info about the parameters
* list a cmdlet's parameters by piping it into `Get-Member`; e.g. `Get-Process | Get-Member` lists all members of the "Get-Process" cmdlet
* use `Get-Help Cmdlet-Name -Examples` to get examples of cmdlet usage
* Use `Get-Help -Name Cmdlet-Name -Full` to get full help on the named cmdlet.
* use `Update-Help` to fetch the latest help files

## Objects
Unlike traditional scripting shells, powershell outputs .Net objects, not plaintext. Objects are underlying types from .Net, but you can create objects using C# or the PSObject type.

Use the `Get-Member` cmdlet to view all properties and methods of a given object.

### Object Piping
The powershell pipeline pipes objects, not text. This means that not all cmdlets can be linked with a pipeline. Read the available help for a given cmdlet to determine what kinds of objects it accepts.


## Pipeline Processing
Conventional PowerShell usage often works by piping objects through a series of cmdlets. This isn't always the most efficient, but it does allow for easily putting together a series of commands to generate desired output.

The following operations can help when generating a pipeline to create desired output.

### Iterating
`ForEach-Object` (aliased to `%`) runs on each object in the pipeline. In common usage, the provided ScriptBlock will be called one each piped object (referenced as `$_` or `$PSItem`) and the returned value will be output along the pipeline.

``` PowerShell
# multiply each value by 5
$values | % { $_ * 5 }
```

This can be used to 

### Filtering
`Where-Object` (aliased to `where`) filters objects out of the pipeline based on a provided ScriptBlock. The ScriptBlock will be called on each piped object (referenced as `$_` or `$PSItem`) and only objects where the ScriptBlock returns a truthy value will continue through the pipeline.

``` PowerShell
# get only values greater than 3
$values | where { $_ -gt 3 }
```

### Sorting
`Sort-Object` (aliased to `sort`) sorts piped objects based on specified properties or even on a scriptblock.

``` PowerShell
# sort primitive values
$values | sort

# sort by a named property
$values | sort 'prop1'

# sort by multiple properties, in order
$values | sort 'prop1', 'prop2'

# sort by calculated value
$values | sort { return $_.prop1 + $_.prop2 }
```

### Formatting
PowerShell reads the default formatting for displaying objects from `C:\Windows\System32\WindowsPowerShell\v1.0\DotNetTypes.format.ps1xml`. This entry may cause some fields not to display by default. If no entry is present for a given type, all properties will be output, in a list (if 5+ properties) or a table (if < 5 properties).

Use the `Format-List`, `Format-Table`, or `Format-Wide` cmdlets to format objects for display.

#### `Format-Wide`
`Format-Wide` takes a collection of objects and displays a single property (`name` by default) of each. Use the `-Property` parameter to specify which property to display. Use the `-Column` parameter to specify how many columns to display in (2 by default).

Useful for a quick overview.

#### `Format-List`
Displays selected properties of all input objects. Use the `-Property` to specify which properties get displayed, using a comma-separated list (`*` for all).

Useful for a detailed view.

#### `Format-Table`
Displays objects in table format, with each object on its own row and each property in a column. This provides a concise way to view numerous objects, but in many cases some properties will need to be omitted or truncated.

The `-Autosize` parameter will attempt to make the data fit within the screen width.

The `-Property` parameter allows customizing what data gets displayed. This parameter takes a list of properties; each property can be specified as a string (naming the property to include), or a HashTable used for calculating the displayed property value. The HashTable format supports keys:
* `Name` (aka `Label`) - mandatory `<string>` - this will head the column for the property
* `Expression` - mandatory `<string>` or `<ScriptBlock>` - the value to display;
    - if `<string>` identifies the object property to display
    - if `<script block>`, the block will get run with `$PSItem` provided to process to generate the display value
* `FormatString` - optional `<string>` - string to use to format the value, run as per the `-f` operator
* `Width` - optional `<int32>` - must be greater than 0
* `Alignment` - optional - value can be Left, Center, or Right



## Snapins
PowerShell snapins are compiled extensions to PowerShell, written via C# or another .NET language. Snapins are largely ignored in favor of modules.

List registered snapins via `Get-PSSnapin -Registered`.

To use a snapin you will need to import it into your current session via `Add-PSSnapin -Name SNAPINNAMEFULLNAME`, presuming the snapin has already been installed.

Use `Get-Command -Module SNAPINNAME` to list the cmdlets available from it.

## Modules
PowerShell modules can be scripted in PowerShell or coded and compiled from a .NET language.

List modules on your system with `Get-Module -ListAvailable`.

You'll need to import a module before you can use it, via `Import-Module -Name MODULENAME`.

Use `Get-Command -Module MODULENAME` to list all commands the module added.

Modules must be in a location described by the `PSModulePath` environment variable.

### Module Auto-Loading
As of PowerShell 3, cmdlets can be used from an external module without being explicitly imported. If the module is on the PSModulePath, it will be imported automatically and the cmdlet will be run, just by invoking its name.



## Input/Output
Use the `Read-Host` cmdlet to get input from the user. E.g. `$MyVar = Read-Host -Prompt 'Enter value for myVar'`.

Use the `Write-Output` cmdlet to write to stdout for display or consumption by another function. E.g. `Write-Output "Hello World!"`.

Use the `Write-Host` cmdlet to write to PowerShell's Information stream (6) specifically for user consumption, e.g. `Write-Host "Doing the thing..."`. Some older documentation caution against this, because PowerShell versions prior to 5.1 did not allow redirection of `Write-Host` output; this is no longer a concern.

`Write-Information` writes to the Information stream for user consumption only if the `$InformationPreference` variable or `-InformationAction` parameter has been set to a value that permits output.

`Write-Verbose` writes to the Verbose stream for user consumption only if the `$VerbosePreference` variable has been set to a value that permtis output or the `-Verbose` switch has been set.

`Write-Warning` writes to the Warning stream.

`Write-Error` writes to the stderr stream; this should generally be used for non-terminating errors, and PowerShell will always display this message with a stack trace to where the `Write-Error` statement was invoked. `throw` terminating errors instead.


## Sources
* [newtrix](https://blog.netwrix.com/2018/02/21/windows-powershell-scripting-tutorial-for-beginners/)
* [howtogeek](https://www.howtogeek.com/141495/geek-school-writing-your-first-full-powershell-script/)