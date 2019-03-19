# Working with Web Services in PowerShell

## Making Web Service Requests
Use the `Invoke-WebRequest` cmdlet to send a web service request. See https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/invoke-webrequest.

Common Parameters:

* `-Body` - object or hashmap which will be converted to json and sent as the body of the request (POST/PUT only)
* `-Method` - type of Http request to use

## Sanitizing URI Elements
Use `[uri]::EscapeDataString($QueryString)` to sanitize the query portion of a URI.

Use `[uri]::EscapeUriString($UriString)` to sanitize an entire URI.
