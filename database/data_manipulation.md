# Data Manipulation
Frequently, you may need to manipulate returned values, or specify values that aren't easy to specify directly as strings.

## Using Strings as Column Names
You can use use a string surrounded by double quotes ('"') to specify a column name. This can be useful for if you wish to use a reserved word as a column name ("TYPE"), or if you want to provide user-readable column aliases ("Item ID").

```SQL
select "TYPE" from SCHEMA.TABLE;

select ID "Item ID" from SCHEMA.TABLE;
```

## Manipulating Strings

### String Concatenation
 Use `||` for concatenation.

### String Functions

* `length` returns the length of a string
* `upper` converts a string to all uppercase
* `lower` converts a string to all lowercase

## Casting Values

### to_number
The `to_number` function converts a non-numeric type to a numeric type. This can be useful for ensuring that strings are sorted as numbers, among other things.

### to_char
The `to_char` function converts a non-string type to a string type.

## Dates
To work with date fields, you may need to be able to generate date values, since coercion of string to date can be unreliable.

### `CURRENT_TIMESTAMP`
Global pseudo-variable `CURRENT_TIMESTAMP` represents the current timestamp.

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