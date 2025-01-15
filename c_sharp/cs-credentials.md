# C-Sharp: Credentials

## Working with the Credential Locker / Password Vault

.NET versions prior to .NET 6 will require the Microsoft.Windows.SDK.Contracts nuget package.

``` C#
// instantiate Vault
Windows.Security.Credentials.PasswordVault Vault = new Windows.Security.Credentials.PasswordVault(); 

// Search by user name (case-sensitive)
var creds = Vault.FindAllByUserName(userName);

// Search by resource name (case-sensitive)
var creds = Vault.FindAllByResource(resource);

// populate the password for the credential (password will be left empty otherwise)
var cred = creds.FirstOrDefault();
cred.RetrievePassword();
// now cred.Password is populated

// store a credential
Vault.Add(new Windows.Security.Credentials.PasswordCredential(resource, userName, password));

// remove a credential (password must be populated)
Vault.Remove(cred);

```

See also: https://learn.microsoft.com/en-us/windows/apps/develop/security/credential-locker