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

## Using `Invoke-Command`
Use `Invoke-Command` to run a specified command on one or more target hosts. `Invoke-Command -ComputerName HOST1,HOST2 -ScriptBlock {Cmdlet-Name -Param1 Arg1}`. Use the `PSComputerName` property to differentiate between outputs for the different hosts.

Use the `$Using:` variable scope in order to reference locally created variables in the remote session. E.g. `$localValue = 5; Invoke-Command -ComputerName HOST1 -ScriptBlock {Cmdlet-Name -Param1 $Using:localValue}`. The remote session is unable to modify the local variables, only to retrieve their value.

The `-ArgumentList` parameter can alternatively be used to pass a list of variables to the remote session, which can then be referenced within the scriptblock either through the `$Args` array or by defining a `param` block that will be populated by based on argument order.

Note that because objects need to be serialized and deserialized during transfer, most methods will no longer be available, and some type conversion may take place, both on local objects passed to the remote session and on remote objects returned.

## Using a PowerShell Session within a script via `Invoke-Command`
Store the session created by `New-PSSession` in a variable, then provided it as the `-Session` parameter to `Invoke-Command` in order to utilize a session within a script.

E.g.:

```PowerShell
$s = New-PSSession -ComputerName Server02 -Credential Domain01\User01
Invoke-Command -Session $s -ScriptBlock {Get-Culture}
```

Successive ScriptBlocks invoked on the same session will run in the same scope, so they can share variables.

