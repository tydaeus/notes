## Variables
Reference a variable's content using the `$` operator. Set a variable with `$VariableName="Variable Value"`.

Delete a variable via `Remove-Item Variable:\VariableName`.

A variable can hold objects or collections of objects. Note that the right-hand side gets fully evaluated prior to assignment. Calling a method directly on a variable holding a collection results in the method being called on all members of the collection, e.g. `$MyProcesses.Kill()` will kill all of MyProcesses. To access a single object, treate the variable as an array, e.g. `$MyProcesses[0]`.

### Data Types
Variables are loosely typed. PowerShell coerces types based on the first variable in an operation.

`$a + $b` will add `a` and `b` if `a` contains a number, or concatenate them if `a` is a string. An error will be thrown if an invalid operation results.

## Get Type of Variable
Use the `GetType` method to determine the type of a variable: `$myVar.GetType()`. This operation is not null safe, so perform a null check first.
