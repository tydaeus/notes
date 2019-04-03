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
