Visual Studio requires manual installation of NUnit3TestAdaptor to support NUnit 3.

Create a project within same solution as AUT using the "Unit Test Project" template.

Conventions:
* Name project same as project being tested with .UnitTests suffix added.
* Name test class same as class being tested + "Tests"
* Name test methods in format `<MethodUnderTest>_<Scenario>_<ExpectedBehavior>`
* Write test statements in Arrange, Act, Assert (equivalent to given, when, then)

NUnit TestRunner looks for classes annotated `[TestFixture]` and methods annotated `[Test]`.

Assertions are provided as static methods on the `Assert` class, e.g. `Assert.IsTrue(result)`. "Plain English" assertion format is also available, e.g. `Assert.That(result, Is.True)` or `Assert.That(result == true)`.

## Setup and TearDown
Method annotated with `[SetUp]` will be called before each test.

Method annotated with `[TearDown]` will be called after each test.

## Parameterized Tests
Annotate parameterized tests with `[TestCase(...)]` specifying the desired values for each test case, then define the test method with corresponding parameters.

``` c#
[Test]
[TestCase(1, 2)]
[TestCase(9, 8)]
public void SomeMethod_WhenCalled_DoesSomething(int a, int b) {...}
// test will be called once with values 1,2 and once with values 9,8
```

Array must be handled as an `object[]` and retrieved as a `params` arg (meaning it must also be the last param), e.g.:

``` c#
[Test]
[TestCase(new object[] { "foo", "bar" })]
public void SomeMethod_WhenCalled_DoesSomething(params string[] args) {...}
```

## Accessing Internals
To allow internal members to be accessed by the tests, you can add the `[assembly: InternalsVisibleTo("AssemblyName")]` attribute to the class under test, where `AssemblyName` is the name of the assembly that can access internal members.

## Testing for Exceptions
``` c#
Assert.Throws<ExceptionType>(() => unit.ExceptionThrowingMethod());
```

## Running from Commandline

Running using nunit3-console (note that test project must be built first):

``` PowerShell
# run all tests in dll
nunit3-console <dllPath>

# run only tests whose name matches TestMethodName (use nUnit-generated method name for exact test, e.g. `TestMethod(5)` for the )
nunit3-console <dllPath> --where="name == <TestMethodName>"

# dotnet equivalent (will find solution automatically if unique in PWD, otherwise solution or project must be specified)
# will build before running
dotnet test [solutionPath] -- NUnit.Where="name == <TestMethodName>"
```

See https://docs.nunit.org/articles/nunit/running-tests/Test-Selection-Language.html for full documentation on test selection.