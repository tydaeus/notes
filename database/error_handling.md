# Error Handling

During PL/SQL execution, most(?) errors will raise an exception, which can potentially be handled, or properly documented.

This document provides condensed instructions for how to perform PL/SQL exception in handling in Oracle. For more info, see [Oracle's documentation](http://docs.oracle.com/cd/B19306_01/appdev.102/b14261/errors.htm#i9355).

## Predefined Exceptions
Oracle defines many exceptions.

### Named Exceptions
Some of the predefined exceptions have names defined for them as part of the SQL/Environment definition. These exceptions can be referenced by their name to raise or handle them.

### Nameless Exceptions
Many predefined exceptions have no name. They must be handled via the `others` handler, unless you define a name for them.

## Handling Exceptions
Exceptions are handled within the `exception` section of a PL/SQL execution block.
```SQL
declare
    ...
begin
    ...
exception
    when EXCEPTION_NAME then -- handles exception named EXCEPTION_NAME
        -- do something
    when OTHER_EXCEPTION or YET_ANOTHER_EXCEPTION then -- handles either
        -- do something
    when OTHERS then -- handles any exceptions not previously handled
        -- do something else
end;
```

### Examining Exceptions
Within the `exception` section, you can use special functions to retrieve information about the caught exception:

* `SQLCODE` retrieves the error code
* `SQLERRM` retrieves the associated error message

## Declaring Exceptions
Declare a variable of type exception;
```SQL
declare
    MY_EXCEPTION exception;
    ...
exceptions
    when MY_EXCEPTION
        -- do something
end;
```

### Naming/Numbering an Exception
Use pragma `exception_init` to associate a declared exception name with a specific error number. Note that pragmas are evaluated at compile time.
```SQL
declare
    NUMBERED_EXCEPTION exception;
    pragma exception_init(NUMBERED_EXCEPTION, -ERR_NUM);
    -- where ERR_NUM is the oracle defined error number
...
```
This allows you to associate a name with an Oracle exception, allowing you to handle or raise the named exception explicitly.

## Raising Exceptions
Use the `raise` operation to raise a named exception:
```SQL
...
    if x < 0 then
        raise MY_EXCEPTION;
    else
        ...
...
```
Note that unless the named exception has been associated with a number via `exception_init`, no SQLCODE or SQLERRM information can be retrieved from it.

Use the `raise_application_error` function as part of a stored procedure or function to specify a custom error number and message that will be available to callers' handlers.

### Re-Raising Exceptions
Using `raise` within an exception block will re-raise the exception so the parent block can handle it:
```SQL
...
    exception
        when MY_EXCEPTION
            raise;
        when OTHERS
            -- do something
    end;
exception
    when MY_EXCEPTION
        -- do something
end;