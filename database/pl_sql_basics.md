# PL/SQL Basics

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

### Variables

#### Declaration
Variables must be declared in the declaration block before they can be used:
```SQL
declare
    /* variableName variableType ; */
    myInt int;
    myString varchar2(20);

```

#### Assignment
Assign to a variable from a select statement with the `select into` statement:
```SQL
    select count(*) into myInt from TABLE_NAME where /* where clause */;
    select
      count(*), max(COLUMN_NAME) into myInt, myOtherVar
    from TABLE_NAME where /* where clause */;
```

#### Referencing
Access the variable within a PL/SQL statement just by using its name:
```SQL
    if myInt < 5 then
    /* ... */
```

### Conditionals

#### If/Then/Else/Elsif
```SQL
    if /* condition */ then
        /* statements */
    elsif /* condition */ then
        /* statements */
    else
        /* statements */
    end if;
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