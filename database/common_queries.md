# Common Queries
Here are some of the recurring, useful query types.

## Using Strings as Column Names
You can use use a string surrounded by double quotes ('"') to specify a column name. This can be useful for if you wish to use a reserved word as a column name ("TYPE"), or if you want to provide user-readable column aliases ("Item ID").

```SQL
select "TYPE" from SCHEMA.TABLE;

select ID "Item ID" from SCHEMA.TABLE;
```

## Casting Values

### to_number
The `to_number` function converts a non-numeric type to a numeric type. This can be useful for ensuring that strings are sorted as numbers, among other things.


## Select Duplicates
To select duplicates by field content, use a group by clause containing the desired field(s).

```sql
select FIELD_1, FIELD_2, count(*) from SCHEMA_NAME.TABLE_NAME
  group by FIELD_1, FIELD_2
  having count(*) > 1
;
```

## Dates
To work with date fields, you may need to be able to generate date values, since coercion of string to date can be unreliable.

### Oracle `to_date`
In Oracle, the `to_date` function is used to convert from string to date. This takes the form of `to_date(dateString, formatString)`, where dateString contains the string to be converted to a date, and formatString contains the string defining the format to use in conversion.

```SQL
to_date('January 15, 1989, 11:00 A.M.', 'Month dd, YYYY, HH:MI A.M.')
to_date('01/15/1989', 'MM/dd/YYYY')
```

#### Format Values
* `Mon` - Month in 3-letter abbreviated form (e.g. 'Nov')
* `MM` - Month in numeric form (e.g. '11')

### Oracle `sysdate`
Oracle provides the sysdate special value for retrieving the current date/time. This date can then be manipulated to create statements relative to the current date.

#### Manipulating days
Syntactic sugar is provided so that adding or subtracting a number from sysdate will change it by that many days.

```SQL
-- the date 35 days ago
select sydate - 35 from dual;
-- the date 5 days from now
select sysdate + 35 from dual;
```

