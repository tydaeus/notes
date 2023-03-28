# Getting Started with Pester

Pester tests are run via `Invoke-Pester`:
* `-Path` specifies the path to the test/tests
* `-Output` specifies level of detail to output

``` PowerShell
Invoke-Pester -Path $PathToMyTest -Output 'Detailed'
```

Pester tests are stored in files named `*.Tests.ps1`; the initial part of the name should indicate the function/unit being tested.

Pester test construction:

```PowerShell
BeforeAll {
    # code to run before testing begins
    # variables are readable but not writeable to tests
    # can also be defined within a Describe or Context block

    # typically we dot-source the test code, ideally separate from the module
    . "$PSScriptRoot\Do-Something.ps1"

    # if we're following the naming convention, we can use a standardized name substitution to find the file
    . ($PSCommandPath -replace '\.Tests\.ps1$', '.ps1')

    # if the script is a function script instead of a function definition
    $scriptPath = $PSCommandPath -replace '\.Tests\.ps1$', '.ps1'
    # (run via &$scriptPath ...
    
    # although modules can work by dot-sourcing the individual scripts, module scoping means testing may 
    # require importing the module, ideally ensuring that it's fresh
    if (Get-Module 'MyModule' -ErrorAction 'Ignore') {
        Remove-Module 'MyModule'
    }
    Import-Module 'MyModule'



}


Describe 'UnitName' {
    # describes one or more tests

    BeforeEach {
        # code to run before each test is run
        # declared variables read/writable within tests
        # can also be defined within nested Describe/Context block
    }

    Context 'when used in this way' {
        # optional nested Describe or Context blocks can subdivide tests further; can nest deeper
        # only difference between describe and context is in output display and when mocking for a specific scope

        It 'behaves this way' {
            # define a specific test

            # setup

            # assertion
            $output | Should -Be $value
        }

        # -TestCases parameter allows defining a list of cases that will be injected into scope and name
        # -ForEach parameter acts as alias to -TestCases
        # if items are hashtables, their properties are directly accessible
        # current item can be referenced as $_ whether hashtable or not
        # Test cases must be literals unless defined in BeforeDiscovery so tests can be generated in Discovery
        It 'does <y> given <x>' -TestCases @(
            @{ x = 5; y = 10 },
            @{ x = 3; y = 6 }
        ) {
            $output = $x * 2
            $output | Should -Be $y
        }

        # -Tag parameter allows specifying one or more arbitrary tags describing the test
        # Invoke-Pester -Tag and/or -ExcludeTag params allow filtering tests by tag
        It 'does something time-intensive' -Tag 'Slow', 'Acceptance' @(
            # does stuff
        )
    }

    # -Skip [switch] parameter indicates that the enclosed tests should not be run
    # Can be passed a boolean variable if defined BeforeDiscovery
    Context -Skip {
        # some tests
    }

    AfterEach {
        # code to run after each test is run
        # declared variables read/writable within tests
        # can also be defined within nested Describe/Context block
    }
}


AfterAll {
    # code to run after testing ends
    # runs within same scope as tests
    # guaranteed to run even if tests fail/crash
    # can also be defined within a Describe or Context block
}

```

## Common Gotchas

- Ensure all setup, teardown, and testing code is in Before*, After*, or It blocks; code outside these blocks will run during Pester's Discovery phase and will likely result in confusing output.
- BeforeDiscovery block can be used to define any processing done before the discovery phase
    + required if using dynamically generated TestCase/ForEach parameters
    + allows custom parameterization of tests