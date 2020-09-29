# Windows Installed Programs
Theoretically, all programs installed to Windows get registered in some manner. In practice, this varies somewhat, but it is possible to get some information about the programs installed. StackOverflow answer https://stackoverflow.com/a/29937569/2939139 provides some of the devilish details.

One common method is to look for uninstallers in the registry, at the following locations:

* `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall`
* `HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Uninstall`
* `HKCU\Software\Microsoft\Windows\CurrentVersion\Uninstall` - remember that this is specific to the current user

The following PowerShell snippet will output info from these keys to a file:

``` PowerShell
(get-childitem hklm:SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall, hklm:SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Uninstall, HKCU:Software\Microsoft\Windows\CurrentVersion\Uninstall).Name |
    % { $path = "Registry::$_"; Get-ItemProperty $path } |
    Select-Object -Property PsChildName, DisplayName, DisplayVersion |
    sort DisplayName |
    Format-Table -AutoSize |
    Out-File C:\temp\installed_files.txt -Width 360
```