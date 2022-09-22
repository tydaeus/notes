# PowerShell Variables
PowerShell variables can hold primitives, objects or collections of objects. Note that the right-hand side gets fully evaluated prior to assignment.

Reference a variable's content using the `$` operator. Set a variable with `$VariableName="Variable Value"`.

Delete a variable via `Remove-Item Variable:\VariableName`.

Calling a method directly on a variable holding a collection results in the method being called on all members of the collection, e.g. `$MyProcesses.Kill()` will kill all members of MyProcesses. To access a single object, treate the variable as an array, e.g. `$MyProcesses[0]`.

## Data Types
Variables are loosely typed. PowerShell coerces types based on the first variable in an operation.

`$a + $b` will add `a` and `b` if `a` contains a number, or concatenate them if `a` is a string. An error will be thrown if an invalid operation results.

### Get Type of Variable
Use the `GetType` method to determine the type of a variable: `$myVar.GetType()`. This operation is not null safe, so perform a null check first.

The `Name` property of a retrieved type provides a string representation, but this isn't necessarily the best way to check types. `FullName` provides a better-qualified option.

Once retrieved, types can then be compared against type literals or each other: `$myVar.GetType() -eq [string]`, `$myVar.GetType() -eq $myOtherVar.GetType()`.

The `-is` and `-isnot` operators can also be used to check typing match, without worrying about null checks: `$myVar -is [string]`.

The `-as` operator will attempt to convert a variable to the specified type, returning `$Null` if unable to do so: `"5/7/07" -as [DateTime]`.

Some caveats about types:
* Arrays are typically `[Object[]]`
* A custom type will not match itself if coming from different declarations, and will generally be unavailable outside its declaring file
* Not all types appear to be automatically referenceable in PowerShell type notation.


## Using the PowerShell `Variable:` Provider

The PowerShell `Variable` provider allows the built-in PowerShell cmdlets to treat variables in a manner similar to filesystem objects.

### Getting a variable's value with `Get-Item`

``` PowerShell
# retrieve variable MyVar
# this will result in a PowerShell error if MyVar doesn't exist; Use -ErrorAction and -ErrorVariable to handle
 Get-Item "Variable:MyVar"
```

Environment variables are under `Env:` instead of `Variable`:

``` PowerShell
Get-Item "Env:Path"
```

`Get-ChildItem` can be used on `Variable:` or `Env:` to list in-scope variables of the specified type.

### Setting a variable with `Set-Item`

``` PowerShell
Set-Item "Variable:MyVar $Value
```

Environment variables use the `Env:` provider:

``` PowerShell
Set-Item "Env:SomeVar" $Value
```

### Deleting a Variable with `Remove-Item`

``` PowerShell
# delete variable MyVar
# this will result in a PowerShell error if MyVar doesn't exist; Use -ErrorAction and -ErrorVariable to handle
Remove-Item "Variable:VariableName"
```



## Using .Net `Environment`

### Setting an Environment Variable
.Net `Environment::SetEnvironmentVariable` can be used to set an environment variable persistently.

``` PowerShell
# set MY_ENV_VAR to "MyVal" for the current process
# this has the same effect as $env:MY_ENV_VAR = 'MyVal'
[Environment]::SetEnvironmentVariable("MY_ENV_VAR", "MyVal")
[Environment]::SetEnvironmentVariable("MY_ENV_VAR", "MyVal", "Process")

# set MY_ENV_VAR to "MyVal" for the current user (persists across sessions, change visible to new processes only)
[Environment]::SetEnvironmentVariable("MY_ENV_VAR", "MyVal", "User")

# set MY_ENV_VAR to "MyVal" for the current machine (persists across sessions, change visible to new processes only)
[Environment]::SetEnvironmentVariable("MY_ENV_VAR", "MyVal", "Machine")
```

As per environment variable conventions setting the environment variable for "User" or "Machine" will not apply to the current process, and no environment variable changes will be applied to other currently running processes.


### Getting an Environment Variable
.Net `Environment::GetEnvironmentVariable` can be used to get an environment variable's value as it is specifically defined for the User, Process, or Machine, with syntax `[Environment]::GetEnvironmentVariable(<VariableName>, [Qualifier])`.