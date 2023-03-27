# Python Exceptions

## `raise`
The `raise` keyword is used in combination with an Error type to raise an error. E.g.:

``` Python
raise ValueError(f"Illegal value specified: {val}")
```

## `try` - `except` - `else` - `finally`
``` python
try:
    # attempt this code
except TypeError:
    # handle the specified exception type (omit type to handle any)
    # multiple except blocks can be used to handle differing types
except ValueError as ex:
    # handle the caught exception via the ex variable
except NotImplementedError:
    # do something, e.g. log
    raise # re-raise the exception, without looking at it
else:
    # do something when no exception occurred
finally:
    # do something regardless of other blocks (including after an unhandled exception)
```

Note that the trace for an unhandled exception will automatically include earlier stages of the exception, even if it has been re-raised as another type.

## Exception Types
All exceptions inherit from `BaseException`.

Catchable exceptions, including user-thrown and -derived exceptions should generally be of type `Exception` or inherit from it.

### Common User-Thrown Exception Types
* `NotImplementedError` - raise to indicate an unimplemented section of code was reached
* `TypeError` - raise to indicate that an object of incorrect type was provided
* `UserWarning` - raise to indicate a warning from user-generated code
* `ValueError` - raise to indicate an illegal value was specified
