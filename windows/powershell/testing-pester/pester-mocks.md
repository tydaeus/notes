# Mocking with Pester

Adapted from https://pester.dev/docs/usage/mocking.

## Basic Mocking
Mock an *existing* function for test-purposes by using the `Mock` function:

``` PowerShell
Mock $functionName
```

Parameters:
* `-CommandName [string]` - the command to be mocked
* `-MockWith [ScriptBlock]` - what to do when the mock is invoked
    - Use `$PesterBoundParameters` to access argument collection; `$PSBoundParameters` will not be available
    - Mocking exit codes:
        + Use `cmd` to provide exit codes, e.g. `{ cmd /c "exit 1" }`
        + ensure that `$global:LASTEXITCODE` gets reset to zero between tests or that "successful" mocks provide a 0 status
* `-Verifiable [switch]` - if set, `Should -InvokeVerifiable` requires that the mock have been invoked
* `-ParameterFilter [ScriptBlock]` - if provided, the mock will be used only when the filter returns true; otherwise the actual/another implementation will be used
    - if mocking PowerShell function with named parameters, the filter block will be provided with the invocation paramters in-scope
    - if mocking non-PowerShell or a function with unnamed parameters, the filter block will need to check `$args`
* `-ModuleName [string]` - name of the module to mock the function's usage within
    - per the documentation, this isn't neccessary for global functions, but testing contradicts in 5.1
* `-RemoveParameterType [string[]]` - list of parameters to skip type enforcement on, allowing looser invocation
* `-RemoveParameterValidation [string[]]` - list of parameters to remove all validation from

You can combine `-MockWith` and `-ParameterFilter` to provide specific outputs corresponding to specific inputs.

## Retaining the Original Command
To reference the original command (e.g. to -MockWith a callthrough to replicate original behavior while allowing invocation testing), use `Get-Command` to retrieve the original command and store it in a variable before creating the mock, then use `&` to invoke the stored version:

``` PowerShell
$originalCommand = Get-Command 'MyCommand'
Mock 'MyCommand' -MockWith { &$originalCommand @args }
```


## Mocking for use by class methods in PowerShell 5.1 or earlier
PowerShell 5.1 and earlier versions aggressively cache commands when used by methods. For this reason, Pester cannot use standard mocking when working with methods. Allegedly, running Pester as a separate job allows mocking of commands used by methods; this doesn't appear to be working in my tests.

I have not found a way to successfully mock invocations initiated from within a PowerShell 5.1 method. It is possible some of my conventions (using a function to call the constructor, constructing via `::new()` instead of `New-Object`) may differ from expected.

Suggested function for doing this provided by Pester:

``` PowerShell
function Invoke-PesterJob
{
[CmdletBinding(DefaultParameterSetName='LegacyOutputXml')]
    param(
        [Parameter(Position=0)]
        [Alias('Path','relative_path')]
        [System.Object[]]
        ${Script},

        [Parameter(Position=1)]
        [Alias('Name')]
        [string[]]
        ${TestName},

        [Parameter(Position=2)]
        [switch]
        ${EnableExit},

        [Parameter(ParameterSetName='LegacyOutputXml', Position=3)]
        [string]
        ${OutputXml},

        [Parameter(Position=4)]
        [Alias('Tags')]
        [string[]]
        ${Tag},

        [string[]]
        ${ExcludeTag},

        [switch]
        ${PassThru},

        [System.Object[]]
        ${CodeCoverage},

        [switch]
        ${Strict},

        [ValidateSet('Diagnostic','Detailed','Normal','Minimal','None')]
        [string]
        ${Output},

        [Parameter(ParameterSetName='NewOutputSet', Mandatory=$true)]
        [string]
        ${OutputFile},

        [Parameter(ParameterSetName='NewOutputSet', Mandatory=$true)]
        [ValidateSet('LegacyNUnitXml','NUnitXml')]
        [string]
        ${OutputFormat},

        [switch]
        ${Quiet}
    )

    $params = $PSBoundParameters

    Start-Job -ScriptBlock { Set-Location $using:pwd; Invoke-Pester @using:params } |
    Receive-Job -Wait -AutoRemoveJob
}
```