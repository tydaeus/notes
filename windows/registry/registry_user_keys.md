# User Registry Keys
The `HKEY_CURRENT_USER` (HKCU) registry keys store configuration and preferences for the current user. These keys are stored within the `HKEY_USERS` tree under the user's SID and synchronized there as part of managing the current user's profile.

The entries under `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProfileList` are organized by SID, so you can determine a user's SID by looking at the `ProfileImagePath` keys for each entry until you find the target user's folder.

Via PowerShell:

```PowerShell
$profile = get-childitem 'HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProfileList' |
    where { $_.GetValue('ProfileImagePath') -match $UserName }
$sid = Split-Path $profile.Name -Leaf
```

Once you have the SID for the user, find their user keys under `HKEY_USERS\${sid}\${normalPath}` where `${normalPath}` is where you'd normally see the key under HKCU. Don't forget that 32-bit applications installed on 64-bit systems will have their keys under `SOFTWARE\WOW6432Node` instead of directly under `SOFTWARE`. 

###### Sources
* http://www.georgealmeida.com/2014/09/how-to-edit-hkey_current_user-for-another-user/