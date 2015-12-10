# Database Structure
Most SQL databases are structured in such a manner that they use tables internally to record the structure of user-defined entities.

## Database Definition Tables
* **user_indexes** stores the definition for the indexes created by the active user
* **user_tables** stores the definition for the tables created by the active user

## DML v. DDL
* **Database Definition Language** is used to *define* the *structure* of the database. This includes CREATE, DROP, ALTER, MODIFY, etc. statements. DDL is committed immediately, and thus cannot be rolled back. To execute DDL within PL/SQL, you must use the `execute immediate` statement.
```SQL
execute immediate 'create index INDEX_NAME on SCHEMA.TABLE(COLUMN)';
```
* **Database Modification Language** is used to *modify* the *contents* of the database. This includes INSERT, DELETE, UPDATE, etc. statements. DML actions are not saved until they are explicitly committed, and thus may be rolled back prior to this point.


