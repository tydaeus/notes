# C# Unit Testing Basics

## Running Unit Tests

### NUnit

``` bat
nunit3-console.exe <test assembly>
```


## Test Coverage

### AltCover

``` bat
cd <build output dir>
altcover.exe
:: instrumented dlls will be in generated __Instrumented subdir, coverage report will be in generated coverage.xml
:: run tests on the dlls placed in the __Instrumented subdir

```

