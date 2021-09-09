# Python Exceptions

## `raise`
The `raise` keyword is used in combination with an Error type to raise an error. E.g.:

``` Python
raise ValueError(f"Illegal value specified: {val}")
```

## Exception Types
All exceptions inherit from `BaseException`.

Catchable exceptions, including user-thrown and -derived exceptions should generally be of type `Exception` or inherit from it.

### Common User-Thrown Exception Types
* `NotImplementedError` - raise to indicate an unimplemented section of code was reached
* `TypeError` - raise to indicate that an object of incorrect type was provided
* `UserWarning` - raise to indicate a warning from user-generated code
* `ValueError` - raise to indicate an illegal value was specified
