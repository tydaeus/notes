#PL/SQL Basics

PL/SQL allows programmatic execution of SQL code.

```SQL
-------------------------------------------------------------------------------
--  Declaration Block                                                        --
--                                                                           --
--  All variables, functions, etc. must be declared here, separated by       --
--  semicolons.                                                              --
-------------------------------------------------------------------------------
declare
    /* variableName variableType */
    myNumber number;
    myString varchar2(20);
    cursor myCursor is
        select /* ... */ ;

-------------------------------------------------------------------------------
--  Execution Block                                                          --
--                                                                           --
--  Statements go here.                                                      --
-------------------------------------------------------------------------------
begin
    /* ... semi-colon separated statements */
end;
```

### Cursors

Cursors allow you to specify a query result that can subsequently be iterated through.

```SQL
declare
    cursor MY_CURSOR is
        select /* select statement */ ;

begin
    for ITERATOR_NAME in MY_CURSOR
    loop
        /* do stuff, using ITERATOR_NAME as a reference to the record currently being processed */;
    end loop;

end;
```