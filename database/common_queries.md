# Common Queries
Here are some of the recurring, useful query types.


## Select Duplicates
To select duplicates by field content, use a group by clause containing the desired field(s).

```sql
select FIELD_1, FIELD_2, count(*) from SCHEMA_NAME.TABLE_NAME
  group by FIELD_1, FIELD_2
  having count(*) > 1
;
```

## Get BLOB Size
To get the byte size of a blob field, use the length function:

```sql
select length(BLOB_COLUMN) from TABLE_NAME;
```

