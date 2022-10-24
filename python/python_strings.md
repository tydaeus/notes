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

## RegEx
Python handles regexes with the built-in `re` module.

See also:
* https://docs.python.org/3/library/re.html
* https://docs.python.org/3/howto/regex.html

RegEx functionality can be accessed either directly through the module or through a RegEx pattern object.

### RegEx Pattern Objects
`re.compile(<string>)` converts a string definition of a regex pattern to a regex object that can then be used for regex operations.

``` Python
cityStateZipPattern = re.compile(r'[^,]+, \w{2} \d{5}(-\d{4})?')
m = cityStateZipPattern.match('SomewheresVille, PA 12345')
```

### RegEx Module-Level Usage
The RegEx module (`re`) provides the same functions as a RegEx pattern object, wherein the first argument specifies the pattern, either as a string or as a compiled RegEx pattern. If a string is specified, it will be compiled and cached before usage, so a RegEx pattern object will generally only outperform usage of the module-level functions if used in a loop.

``` Python
m = re.match(r'[^,]+, \w{2} \d{5}(-\d{4})?', 'SomewheresVille, PA 12345')
```

### Python-Specific Regex

#### Named Groups
You can name a capture group by beginning it with `?<group_name>`, e.g.:

``` Python
m = re.match(r'(?<city>[^,]+), (?<state>\w{2}) (?<zip>\d{5}(-\d{4})?)', 'SomewheresVille, PA 12345')
```


### RegEx Functions

**Matching Object Functions** return `None` if no match, or a match object if a match is found. 
- `match(<string>, startindex = 0[, numchars])` looks for a match at the start of the specified interval
- `search(<string>, startindex = 0[], numchars])` looks for a match anywhere within the specified interval.


**Find Functions** return the string portions matched by the regex.
- `findall(<string>)` method returns a list of the strings matching the regex
- `finditer(<string>)` returns an iterator for retrieving the strings matching the regex

`re.escape(<string>)` applies regex escaping to all regex pattern special characters within the string.

#### Substitution Functions
These regex functions use the regex pattern to find and replace.

`re.sub(pattern, repl, str, count=0, flags=0)` finds and replaces `pattern` within `str` with `repl`.

* `count` - if non-zero will be the maximum number of replacements to occur
* `flags` - allows specifying regex flags (specified as RegexFlag instances defined in `re`)


### RegEx Match Objects
Use the returned match object to get info about the matching results.

`match.expand(<template>)` performs backslash substitution on the match using the specified template, with `\1`, `\2` replacing specified groups (`\g<name>`, including angle brackets, to replace a named group), returning the result.

`match.group(<group reference>)` retrieves the specified group by index, or by name if using named groups (indexes still available for named groups). Group 0 is the entire match. The `[]` operator can be used to do this too.

`match.groups(defaultval = None)` retrieves all matched groups as a tuple, using `defaultval` in place of any missing (optional) groups.

`match.groupdict(defaultval = None)` retrieves all matched named groups as a dict, using `defaultval` in place of any missing (optional) groups.


