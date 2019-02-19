# Working with Credentials in PowerShell
PowerShell includes some good functionality for working with credentials.

Sources:

* https://blog.kloud.com.au/2016/04/21/using-saved-credentials-securely-in-powershell-scripts/
* https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.security/get-credential?view=powershell-6
* https://stackoverflow.com/questions/818704/how-to-convert-securestring-to-system-string

## Getting Credential Info from User
Use the `Get-Credential` commandlet to get a username and password from the user, e.g. `$authCredentials = Get-Credential`.

The resulting credential object can be passed to a number of different commandlets using the `-Credential` parameter.

The `Password` property of the credential represents the password as a `System.Security.SecureString`. If needed, this can be written to a file to store it, or separated and reconverted to plaintext for purposes of using it, e.g. in a web request.

## Working With SecureStrings

### Store and Retrieve SecureString Securely
Writing a SecureString to a file:

``` PowerShell
$secureStringText = $secureString | ConvertFrom-SecureString
Set-Content $pathToFile $secureStringText
```

Reading the SecureString back from the file:

``` PowerShell
$secureStringText = Get-Content $pathToFile
$secureString = $secureStringText | ConvertTo-SecureString
```

This process uses the Windows Data Protection API (DAPI) to encrypt and decrypt the secureString's content with a key that is unique to the current user/computer combination.

### Convert SecureString to Plaintext Content
Do not do this unless you absolutely have to, because it isn't a very secure way of handling secure data.

``` PowerShell
[System.Net.NetworkCredential]::new('', $SecureString).Password
```
