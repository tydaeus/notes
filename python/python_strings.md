# Python Strings

## String Formatting
Strings can be wrapped in `'`, `"`, or `"""`; `"""` supports multi-line strings.

There are multiple options for string formatting.

`print()` supports C-style `%` annotated string formatting: 
``` Python
print("""
intVal:%i;
floatVal:%.2f;
stringVal:%s;
""" % (myInt, myFloat, myString))
```

The `format` method of the string object can be used to perform ordered or labelled replacements:

``` Python
print('myInt:{0}, myString:{1}'.format(myInt, myString))
print('myInt:{intVal}, myString:{stringVal}'.format(intVal=myInt, stringVal=myString))
```

Python 3.6+ supports inline string interpolation by prefixing the string with `f`:

``` Python
print(f'myInt:{myInt}, myString:{myString}')
```

## Working with Strings

`.startswith(str)` returns whether a string starts with the specified string
`.endswith(str)`
`.count(str)` count occurrences of specified string within string
`.upper()`
`.lower()`

`==` is overloaded to perform string equality checking. Comparison operators are also overloaded.

`.find(str)` returns index of specified string - returns -1 if not found
`.index(str)` returns index of specified string - throws Error if not found
`.split(str)` splits a string into a list of strings using the specified separator
`.join(list)` uses the calling string as a separator to join the specified list
`.isdigit()` returns whether the string contains only digits
`.isalpha()` returns whether the string contains only alphabet characters

`.isupper()` returns whether string is completely uppercase
