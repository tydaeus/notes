# Database Structure

## Database Definition Tables
Most SQL databases are structured in such a manner that they use tables internally to record the structure of user-defined entities.

Within Oracle, and possibly others, the structural tables follow a specific naming convention.

#### Prefixes
The names of structural tables begin with a prefix indicating the scope covered by the table, followed by '_'.
* **user**-prefixed tables store data about the tables owned by the current user/schema
* **all**-prefixed tables store data about all tables accessible to the current user
* **dba**-prefixed tables store data about all tables in the database

#### Structure Tables
* **indexes** stores the definition for indexes
* **tables** stores the definition for the tables
* **tab_cols** stores the definition of table columns

So, for example, `user_tab_cols` would contain the definition of the columns belonging to tables owned by the current user/schema.

## DML v. DDL
* **Database Definition Language** is used to *define* the *structure* of the database. This includes CREATE, DROP, ALTER, MODIFY, etc. statements. DDL is committed immediately, and thus cannot be rolled back. To execute DDL within PL/SQL, you must use the `execute immediate` statement.
```SQL
execute immediate 'create index INDEX_NAME on SCHEMA.TABLE(COLUMN)';
```
* **Database Modification Language** is used to *modify* the *contents* of the database. This includes INSERT, DELETE, UPDATE, etc. statements. DML actions are not saved until they are explicitly committed, and thus may be rolled back prior to this point.


