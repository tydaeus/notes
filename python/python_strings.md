# Python Strings
Strings can be wrapped in `'`, `"`, or `"""`; `"""` supports multi-line strings.

Prefixing a string literal with `r` designates it as a raw string, not subject to escape characters; e.g. `'\n'` is a newline, while `r'\n'` is '\' followed by 'n'.

Strings are managed internally as an immutable collection (similar to a tuple), and the indexing and slicing operations can be used to reference their contents.

The comparison operators are overloaded to perform string equality checking and comparison. 


## String Formatting

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

## String Methods

* `.startswith(str)` returns whether a string starts with the specified string
* `.endswith(str)`
* `.count(str)` count occurrences of specified string within string
* `.upper()`
* `.lower()`
* `.find(str)` returns index of specified string - returns -1 if not found
* `.index(str)` returns index of specified string - throws Error if not found
* `.split(str)` splits a string into a list of strings using the specified separator
* `.join(list)` uses the calling string as a separator to join the specified list
* `.isdigit()` returns whether the string contains only digits
* `.isalpha()` returns whether the string contains only alphabet characters
* `.isupper()` returns whether string is completely uppercase
