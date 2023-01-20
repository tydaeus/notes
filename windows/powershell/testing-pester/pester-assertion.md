# Pester Assertions

## `Should`
The `Should` function is the heart of Pester assertions.

``` PowerShell
$actual | Should -Be $expected
```

Primary parameters:
* `-Be` - equality (case-insensitive); overloaded to compare arrays element by element
* `-BeExactly` - equality (case-sensitive)
* `-BeFalse` - value is false or falsey
* `-BeGreaterOrEqual` - comparison via `-ge`
* `-BeGreaterThan` - comparison via `-gt`
* `-BeIn` - value is present in array/collection
* `-BeLessOrEqual` - comparison via `-le`
* `-BeLessThan` - comparison via `-lt`
* `-BeLike` - wildcard matching via `-like`
* `-BeLikeExactly` - wildcard matching via `-clike`
* `-BeNullOrEmpty` - string null or empty check using `[string]::IsNullOrEmpty()`
* `-BeOfType` - type checking via `-is`; does not work with custom type names
* `-BeTrue` - value is true or truthy
* `-Contain` - collection contains value via `-contains`
* `-Exist` - is the object present in a PS provider? Similar to `Test-Path`.
* `-FileContentMatch` - check if file (specified by Item or path string) contains specified test, matching using case-insensitive regex
* `-FileContentMatchExactly` - check if file contains specified text using case-sensitive regex
* `-FileContentMatchMultiline` - check file for specified text using case-insensitive regex, treating whole file as one string
    - does not appear able to assert file is empty
* `-HaveCount` - collection has expected item count
* `-HaveParameter` - checks that function parameter exists; can further qualify:
    + `-Type`
    + `-DefaultValue`
    + `-Mandatory`
* `-Invoke` - verify that mocks have been called
    * `-CommandName` - name of mocked command
    * `-Times` - number of times it should have been called
    * `-ParameterFilter` (optional) - filter parameters invoked with for counting purposes
* `-InvokeVerifiable` - verify that all mocks constructed with `-Verifiable` have been called
* `-Match` - objects match via `-match`
* `-MatchExactly` - objects match via `-cmatch`
* `-Throw` - expect an exception to be thrown; optionally specify `[string]` message of exception for `-like` matching
    + input must be `[scriptblock]` to allow wrapping
    + can set `-ErrorAction Stop` during invocation to cause stderr messages to result in a throw to check errors that are not normally terminating

Modifying parameters:
* `-Not` - negation; sensitive on positioning (`Should -Not -HaveParameter 'Foo' -Mandatory` vs. `Should -HaveParameter 'Foo' -Not -Mandatory`)
* `-ErrorActionPreference [string]` - defaults to 'Stop' unless configured otherwise; set to 'Continue' to allow testing to continue current test if this assertion fails

Other parameters:
* `-Because [string]` - add optional message to assertion failure message explaining why test should pass