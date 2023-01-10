# Pester - Installation

Adapted from https://pester.dev/docs/introduction/installation.

Pester 3.4.0 comes pre-installed in Win10, complicating the install of newer versions of Pester.

## To Install
1. (if TLS 1.2 is not default)
    ``` PowerShell
    [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor [System.Net.SecurityProtocolType]::Tls12
    ```
2. (if Pester 3.4.0 already installed)
    ``` PowerShell
    Install-Module -Name Pester -Force -SkipPublisherCheck
    ```
3. 

## To Import Explicitly

``` PowerShell
Import-Module -Name 'Pester' -MinimumVersion '4.0.0.0'
```

(not needed normally, unless you're wanting to explicitly call an older or specific version)