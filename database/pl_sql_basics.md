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
/   -- PL/SQL statements can contain ';', so '/' or 'RUN' must be used after
    -- they have been completely entered.
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
/
```

### Text Output

Use `dbms_output.put_line('text here')` to trigger text output. Depending on client, you may also need to enable the display of this output. SQLDeveloper requires the DBMS Output view to be displayed and a connection to be created between this view and the database. Use `||` for concatenation, and the `to_char()` function to convert non-string data to printable format.