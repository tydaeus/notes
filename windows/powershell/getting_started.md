# Getting Started with Powershell
Notes on first-time powershell, based on [this tutorial](https://blog.netwrix.com/2018/02/21/windows-powershell-scripting-tutorial-for-beginners/).

## Starting Powershell
Start `powershell.exe`, or `powershell_ise.exe`. The second option is an interactive scripting environment, and thus more useful for script development.

## Running Scripts
Scripts are conventionally stored in `.ps1` files. Run the script by right-clicking and selecting `Run with PowerShell`.

## Setting ExecutionPolicy
Use `Get-ExecutionPolicy` to check what the current execution policy is. This should get set to RemoteSigned via `Set-ExecutionPolicy RemoteSigned` if it is not already set to RemoteSigned.

## About Cmdlets
Cmdlets are the powershell command functions. About cmdlets:

* system, user, and custom cmdlets are available
* output as an object or array of objects
* can retrieve data or pipe it
* names are case-insensitive
* named in verb-noun format, where the verb defines what is done and the noun defines what it's being done to
* use `Get-Help -Category cmdlet` to list all cmdlets
* can accept parameters in `-ParameterName ParameterValue` format; e.g. `Get-Service -Name W*`
* may have an alias; use `Get-Alias` to list all aliases

## Getting Help
* use `Get-Help -Category cmdlet` to list all cmdlets
* list a cmdlet's parameters by piping it into `Get-Member`; e.g. `Get-Process | Get-Member` lists all members of the "Get-Process" cmdlet
* use `Get-Help Cmdlet-Name -Examples` to get examples of cmdlet usage

## Comments
Use `#` for comment lines.

Use `<#` and `#>` for block comments.
