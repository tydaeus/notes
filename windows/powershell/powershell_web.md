# Working with Web Services in PowerShell

## Making Web Service Requests
Use the `Invoke-WebRequest` cmdlet to send a web service request. See https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/invoke-webrequest.

Common Parameters:

* `-Body` - object or hashmap which will be converted to json and sent as the body of the request (POST/PUT only)
* `-Method` - type of Http request to use

## Sanitizing URI Elements
Use `[uri]::EscapeDataString($QueryString)` to sanitize the query portion of a URI.

Use `[uri]::EscapeUriString($UriString)` to sanitize an entire URI.

## Supporting Web Protocols
`[System.Net.ServicePointManager]::SecurityProtocol` is a list of `[System.Net.SecurityProtocolType]` enums that determines what protocols are available for .NET (including Invoke-WebRequest and other PowerShell cmdlets).

E.g. to set support to TLS1.2 only, use: `[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.SecurityProtocolType]::Tls12`

## Basic Authentication
For completely standard Basic Authentication, use the `-Credential` parameter of `Invoke-WebRequest`, e.g.:

``` PowerShell
$credential = Get-Credential
Invoke-WebRequest -Uri $targetUri -Credential $credential
```

For services that use non-standard Basic Authentication (e.g. for token login), you will need to manually construct the request header:

``` PowerShell
$login = "$($userName):$($password)"

$encodedLogin = [System.Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($login))

$authorizationHeader = "Basic $encodedLogin"

$Headers = @{
    Authorization = $authorizationHeader
}

Invoke-WebRequest -Uri $targetUri -Headers $Headers
```

(from https://stackoverflow.com/a/27951845/2939139)