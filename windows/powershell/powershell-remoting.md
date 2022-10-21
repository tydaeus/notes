# PowerShell Remoting
PowerShell can run commands on remote hosts using the WinRM Service.

## Enable Remoting
Run `Enable-PSRemoting` to enable remoting on the machine you want to connect to. Answer `yes` to all prompts. Under Windows 7, enabling will fail if your NIC's location is set to Public. Switch to the Home or Work network location, or skip the network check via `Enable-PSRemoting -SkipNetworkProfileCheck`.

You may also need to enable read and execute privileges:

1. Open PowerShell as Administrator
2. Execute command: `Set-PSSessionConfiguration -Name Microsoft.PowerShell -ShowSecurityDescriptorUI`
3. Ensure the user (or their group) has `Read` and `Execute` permissions.

## Using a PowerShell Session
Use `Enter-PSSession -ComputerName "HOSTNAME"` to start a PowerShell session on HOSTNAME. You now have a command-prompt that can be used to run PowerShell commands.

Use `Exit-PSSession` to exit the PSSession when done.

## Remote Invocation Parameters
Many PowerShell commands provide parameters that allow specifying a run in a remote session. 

* `ComputerName` allows specifying the name of one or more target hosts as a `[string[]]`
* `Session` allows specifying one ore more already initialized sessions as a `[PSSession[]]`

### `Invoke-Command`
Use `Invoke-Command` allows running a ScriptBlock locally or on one or more remote hosts.

Use the `$Using:` variable scope within the ScriptBlock in order to reference locally created variables in the remote session. E.g. `$localValue = 5; Invoke-Command -ComputerName HOST1 -ScriptBlock {Cmdlet-Name -Param1 $Using:localValue}`. The remote session is unable to modify the local variables, only to retrieve their value.

The `-ArgumentList` parameter can alternatively be used to pass a list of variables to the remote session, which can then be referenced within the scriptblock either through the `$Args` array or by defining a `param` block that will be populated by based on argument order.

#### Returned Data
Because objects need to be serialized and deserialized during transfer, most methods will no longer be available on returned objects, and some type conversion may take place, both on local objects passed to the remote session and on remote objects returned.

The `PSComputerName` property will be populated on returned objects to indicate the computer they were returned from. Depending on object type and display methodology, there may be times when this property displays when you might not expect it to.

The exact deserialization process varies depending on the object types in question. Primitives, strings, and `[string[]]` types will be preserved.

Be aware that streamed output in string form will run together if run on multiple hosts, so it may be desirable to have the ScriptBlock bundle it into a `[PSCustomObject]` so that each invocation of the ScriptBlock is separated.


### Using a PowerShell Session within a script via `Invoke-Command`
Store the session created by `New-PSSession` in a variable, then provide it as the `-Session` parameter to operate within that session. This variable will be of type `[System.Management.Automation.Runspaces.PSSession]`

E.g.:

```PowerShell
$s = New-PSSession -ComputerName Server02 -Credential Domain01\User01
Invoke-Command -Session $s -ScriptBlock {Get-Culture}
```

Successive commands invoked on the same session will run in the same scope, so they can share variables.

