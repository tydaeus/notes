# Oracle-Specific SQL

### '/' vs ';' vs 'RUN;'

**';'** is used to terminate standard SQL statements.

SQL statements get stored in a single-command buffer, as they are entered. Standard SQL statements will get executed once the ';' has been entered.

```SQL
select * from SCHEMA.TABLE;
-- results display
```

**'RUN;'** will cause the statement currently in the buffer to be displayed and then executed.

```SQL
select * from SCHEMA.TABLE;
-- results display
run;
-- command displays
-- results display
```

**'/'** alone at the beginning of a new line will cause the statement currently in the buffer to be executed, without being displayed.

```SQL
select * from SCHEMA.TABLE;
-- results display
/
-- results display
select * from SCHEMA.TABLE/
-- results display
```

Using both '/' and ';' on a standard SQL statement will cause it to run twice.
```SQL
-- Don't do this!
select * from SCHEMA.TABLE;
/
-- results display
-- results display again
```

**PL/SQL** statements can contain ';' within them, because they are groups of standard and PL/SQL commands. Thus, **'/'** or **'RUN;'** *must* be called to execute them after they have been completely entered into the buffer.

SQL*Plus commands are run by the SQL*Plus client, and should have neither ';' nor '/'; '/' will cause the last SQL command to be run again.