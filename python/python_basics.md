# Python Basics
Rough notes on the basics of Python. Note that names used for placeholder variables may not (yet) correspond to the official parameter names; this was a adapted from a tutorial instead of from the Python references.

## Base Syntax and Conventions
Statements are assumed to be single-line unless obviously otherwise. Use `\` as the last character of a line to explicitly indicate continuation of the statement.

Use `;` to separate multiple statements on a single line.

Block statements are delineated with `:` to begin the block, and indentation of successive lines to indicate continuation of the block. Python is **sensitive to the level of indentation**, so do not use arbitrary formatting.

Python identifier convention:
* all-lowercase snake-case for user-defined identifiers, e.g. `my_var`, `is_in_usa()`
* all-lowercase without a divider for built-in identifiers, e.g. `.startswith()`, `.endswith()` (built-in identifiers seem to prefer single-word when possible)

## Operators
In addition to standard C-family operators, Python supports:

* `//` - integer division
* `**` - exponentiation

Python does **not** support the increment (`++`) and decrement (`--`) operators.

Python also overloads the following operators for string operations:

* `+` - concatenation (string + string)
* `*` - repetition (string * integer)
* comparison operators
* `[]` - is parameter sensitive:
    - `str[index]` - returns the character at `index`
    - `str[startIndex:endIndex]` - returns all characters from `startIndex` (inclusive) to `endIndex` (exclusive)
    - `str[startIndex:]` - returns all characters after `startIndex`
    - `str[:numChars]` - returns the first `numChars` characters

## Keywords

* `del` - deletes an object manually
* `pass` - does nothing (occasionally useful for readability)

## Special Values

* `None` - represents the absence of a value

## Built-In Functions

* `len(<string String>)` - returns the length of a string
* `type(<any Value>)` - returns the type of its argument
* `print(<string Output>)` - outputs a string to the console
* `input(<string Prompt>)` - displays `Prompt` as a prompt for input, returns the resulting input (as a string)
* `min()`, `max()`, and `round()`
* `hex(num)` - returns string representation of `num` in hexadecimal
* `oct(num)` - returns string representation of `num` in octal
* `bin(num)` - returns string representation of `num` in binary

### Type Conversion Functions
* `float(var)`
* `int(var[, base])` - decimal values will be truncated, strings will be converted as base 10 unless base is specified; specify base 0 to convert based on string prefix
* `str(var)`
* `list(var)` - convert to a list (strings will be converted into a list of letters), non-iterables will result in error; does not apply recursively
* `tuple(var)` - convert to a tuple (strings will be converted to a tuple of letters), non-iterables will result in error; does not apply recursively
* `dict(var)` - convert to a dictionary; must be paired collections, and first item in pair must be hashable
* `set(var)` - convert to a set; removes duplicates, must be iterable, all elements must be hashable
* `ord(char)` - returns unicode integer value of `char`
* `chr(num)` - returns the unicode character corresponding to numeric value `num`
* `complex(num)` - returns the complex number representation of the specified number; note that `j` is used in place of `i` as the imaginary coefficient



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

## Lists
Lists are a built-in data structure in Python, representing an ordered collection of objects.

Lists are denoted by surrounding with `[]` and separating elements with `,`.

E.g. `['foo', 1, False]`

Reference list elements using `[]` after var name, 0-indexed: `myList[0]` returns the first element.

Python supports negative index to access elements relative to the end of the list, so `myList[-1]` returns the last element.

Note that strings operate as lists of characters, so most list operations will work on strings -- but strings are immutable.

### List Operations

Use the `.append(<item>)` method to add one element to the end of a list.

Use `.insert(<index>, <item>)` method to insert an element at the specified index of the list.

Use `.extend(<list>)` to add another list onto the end of a list; `+=` is overloaded to support this

Use the `+` operator to combine two lists and return a new list comprised of the elements of both lists.

Use `.index(<item>)` to get the index of the first element that is equal to `item`; note that Python is case-sensitive. An error will be thrown if the item is not found.

The `in` operator returns whether a matching element is present in the list: `x in my_list`

Use `.remove(<item>)` to remove the first element that is equal to `item`.

Use `.sort()` to sort the list (ascending by default).

Use `.reverse()` to reverse the order of the list (in-place).

`.pop([index])` removes the specified element, defaulting to the last element if index is not specified. Negative indexing works from the end of the list.

`.count(<item>)` counts matching elements.

`.clear()` empties the list.

`.copy()` returns a deep (non-recursive?) copy of the list

### Built-in Functions and lists

`set(<list>)` - returns a set constructed from the list's contents.

`max()` and `min()` work with a list as argument

`len(<list>)` gets the length of a list.

`sorted(<list>)` returns a sorted copy of a list.

`sum(<list>)` adds the elements

`*` (`<list> * <int>`) and `*=` (`<list> *= <int>`) are overloaded to repeat the lists elements.

`all(<list>)` returns whether all elements in the list are truthy; an empty list is true.

`any(<list>)` returns whether any elements in the list are truthy; an empty list is false.

`.join(str)` joins a list of strings using the specified string between the strings

### List Slicing
Slicing allows accessing a range of elements. Slicing returns a copy of the list, not a reference.

`myList[<start_index>:<end_index>]` returns list elements starting with the element at `start_index` up to but not including the element at `end_index`.

A slice starting after the end of the list returns an empty list.

Drop `end_index` to get all elements after `start_index`.

Dropping `start_index` starts at index 0.

You can also slice with negative indexes; watch out for confusions. `my_list[0:-3]` returns elements from the start of the list up to but not including the element 3 positions from the end. An empty list will be returned if you specify an invalid range (e.g. `[-3:-1]`).

#### Step Size
The optional step-size parameter allows for skipping list elements when slicing; e.g. a step-size of 2 will retrieve every other element.

`['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm'][::2]` returns `['a','c','e','g','i','l','m']`.

Negative step size results in stepping through the list in reversed order.

#### Adding to a List via Slice Positioning
You can add elements to a list via slice positioning notation. This can operate even more counter-intuitively than other slicing, so be sure to test.

`my_list[2:-1] = ['a', 'b']` will modify `my_list` to use elements `'a','b'` to replace the elements from index 2 up to but not including the last element.

`my_list[2:0] = ['a', 'b']` inserts `'a','b'` at index 2 without overwriting any.

`my_list[2:] = ['a', 'b']` replaces elements from index 2 to the end of the list with `'a','b'`.

#### Unpacking a List
You can assign a comma-separated series of values as equal to a list, and Python will automatically assign corresponding elements of the list to the variables.

`a, b, c, d = my_list`

Note that the number of variables must be equal to the number of elements in the list. Specify a throwaway variable (`_` by convention) to ignore values from the list.


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


## Tuples
Tuples are an ordred collection of items, initialized in `()`.

`+` is overloaded to combine tuples
`*` is overloaded to repeat a tuple an integer number of times

Python will automatically coerce comma-separated items into a tuple.

Unpacking works with tuples.

Access tuple fields using the same subscript notation as used for lists.

Tuples also support slicing.

Use the `in` character to check if an element is present. (`not in` for negation)

Use the `list()` built-in function to convert to a list.

Use the `set()` built-in function to convert to a set.

Use the `tuple()` built-in function to convert a list to a tuple.

The main difference from lists is that tuples are immutable.

### Zipping

The `zip()` function pairs elements from two collections into a collection of tupled paired values. Convert to a tuple, list, set, or dictionary to interact with the zipped output. Unpaired values are discarded within the accessor collection, but persist when the object is unzipped.

Unzip via unpacking: `x, y = zip(*zip_formatted_collection)`

Zipping/unzipping is a convenient way for working with collections that correspond with each other (e.g. x-y coordinates).

## Dictionaries
Dictionaries store records, each of which is a key-value pair. Initialize in braces `{}` with comma-separated key:value pairs:

```Python
my_dictionary = {
    'foo':'bar',
    'myVal':2
}
```

```Python
my_dict = {}
```

Note that keys do not need to be strings, and will not be coerced into strings. (Boolean values will be coerced into integers, however).

Duplicate keys specified during initialization will result in only the last specified pairing.

Reference a record by looking up the key: `my_dict[key]`.

`.keys()` returns the collection of keys in the dictionary; use in combination with the `in` keyword to check if a key is present: `key in my_dict.keys()`.

`.values()` returns the collection of values in the dictionary.

`del` can be used to remove keys: `del my_dict[key]` will remove the specified key. 

`len(dict)` returns the number of key-value pairs

`sorted(dict)` returns a sorted list of the dictionary's keys (keys must be of comparable types)

`.items()` returns the key-value pairs as a list of tuple pairs.

`.copy()` returns a copy

`.pop(key)` returns the value of the specified key and removes it from the dictionary.

`.popitem()` returns an arbitrary key-value pair as a tuple and removes it from the dictionary.

`.update(dict)` updates the calling dictionary with the keys and values from the specified dictionary

`.clear()` empties a dictionary.

`dict()` can be used to convert a collection of ordered pairs into a dictionary; the first elements will be keys, and the second elements will be values.

## Sets
A set is an unordered collection of unique elements. Initialize in braces `{}`:

``` Python
my_set = {1, 2, 3, 4, 5}
empty_set = set()
```

Note that duplicates can be specified at initialization, and will be discarded automatically.

Sets can only contain immutable elements (otherwise they couldn't ensure that elements are unique).

`.add(element)` adds the specified element to the set (if not already present) -- no error if it is a duplicate.

`len(set)` returns the number of elements.

`max(set)` and `min(set)` can be used.

`.discard(element)` removes the specified element if present, does not throw an Error if absent.

`.remove(element)` removes the specified element if present, throws an Error if absent.

`.clear()` empties the set.

`.union(set...)` returns the union of the calling set with specified set(s).

`.intersection(set)` returns the intersection set between the calling set and the specified set.

`.difference(set)` returns elements in the calling set not present in the specified set.

`.intersection_update(set)` populates the calling set with the intersection of the calling set and the specified set. `_update` variants also exist for intersection and difference.

`.isdisjoint(set)` returns whether there are no elements in common between the sets.

`.issubset(set)`

`.issuperset(set)`

## Copying

Use `import copy` to import the 'copy' library.

`copy.copy(object)` returns a shallow copy of the specified object.

`copy.deepcopy(object)` returns a deep (recursive) copy of the specified object.

## Conditionals
Python supports the standard C-family comparison operators.

`and`, `or`, and `not` perform standard boolean logic.

### `if` statements

``` Python
if bool_condition:
    do_action1()
    do_action2()
elif bool_condition2:
    do_action3()
else:
    do_action4()
```

**Important**: Python uses indent level, not wrapping symbols, to indicate statement nesting level.

Ternary operator:

```Python
result = true_result if bool_condtion else false_result
```

## For Loops

### `for in` Loops
`for in` loop makes it easy to loop over an iterable type.

``` Python
for elem in my_iterable:
    do_something()
```

Iterating through a dictionary iterates through its keys; use the `.values()` or `.items()` method to iterate through values or ittems respectively. You can unpack the items to have access to both key and value:

``` Python
for key, value in my_dict.items():
    print(f"Key: {key} = Value: {value}")
```

#### Iterating Through Nested Iterables with `enumerate`
The `enumerate()` built-in function allows capturing nested elements within tuply-specified identifiers.

``` Python
my_collection = (
    (1, 2),
    (3, 4),
    (5, 6)
)

for i, (x, y) in enumerate(my_collection):
    print(f"Pair {i} is: {x}, {y})

# Output:
#   Pair 0 is: 1, 2
#   Pair 1 is: 3, 4
#   Pair 3 is: 5, 6
```


### `for`-`else` Block
A `for` loop can contain an `else` block. The `else` block will be run once the loop has completed, unless it is terminated prematurely/abnormally.


### `break` statement
Use the `break` keyword to terminate a loop prematurely. Note that this will prevent the `else` block from running, making the `else` block a convenient way to run code in cases where something wasn't found within the loop. 

### `continue` statement
Use the `continue` keyword to skip the remainder of the loop body, then run the next iteration of the loop.

### Comprehensions
Comprehensions are a single-line invocation of `for` that allows for easily initializing a data structure from an iterable. This takes the form of `[Transformation for Iterator in Iterable]`.

``` Python
# generate a list of 0**3 to 9**3
my_list = [i ** 3 for i in range(10)]
```

Add an if clause to filter:

``` Python
# generate a list of only odd numbers cubed from 0 to 9
my_list = [i ** 3 for i in range(10) if i % 2 != 0]
```

Additional `if` clauses can be added to filter further.

`if` clauses can also be used within the transformation clause to allow for conditional logic.

Sets, dictionaries, and tuples can also be created via Comprehension, by changing the wrapping characters.



## `range()`
- `range(end_int)` returns a range object representing the integer numbers from 0 to (end_int - 1)
- `range(start_int, end_int)` returns a range object representing integer numbers from start_int to (end_int - 1)
- `range(start_int, end_int, interval)` returns a range object representing integer numbers from start_int to (end_int - 1) with the specified interval between numbers

The `range()` function is the standard way for Python to iterate over a series of numbers:

``` Python
for i in range(5):
    # Do something
```

Use `reversed()` to reverse the order of the range.

## `while` loops

``` Python
while <condition>:
    # do something
```

Note that condition evaluation is based on whether value is Truthy, rather than strict boolean True.

### Single-Line `while`

``` Python
while(<condition>): # do something
```

### `while`-`else`
Similar to with a `for` loop, a `while` loop can optionally include an `else` block which will run if the loop terminates normally.



## Functions
Use the `def` keyword to define a function. Functions are only available after they have been defined. Syntax errors will not be detected until the function is invoked.

``` Python
def my_function():
    # statements

# call defined function
my_function()
```

Python functions support recursion, up to `sys.getrecursionlimit()` depth (`sys` module required). Use `sys.setrecurionslimit(limit)` to change the limit; note that buffering etc. may prevent output from deeper in the recursion stack.

Functions are represented as objects in Python. As such they can be assigned (by reference) to variables and passed around.

The first string include before any code statement in a function body but not assigned to a variable will be treated as in-line documentation, accessible from the function's `__doc__` property. Multi-line (triple-quoted) strings are ideal for this purpose.

``` Python
def my_function():
    """\
    Inline Documentation

    Some more info. \
    """
    # do stuff
```

Use the `return` keyword to return a value. Multiple values can be returned by separating them with a comma; this returns the values in a tuple, which can be unpacked into multiple values.

### Function Parameters
Provide parameter names in the function definition to define it as a parameter.

``` Python
def my_function(param_1, param_2):
    # do stuff

# invoke with positional parameters
my_function(val_1, val_2)
```

Parameters are untyped; use manual typing for error checking or to allow type-based overloading.

#### Keyword Arguments
Specify a parameter name and value in `<parameter>=<value>` syntax to pass an argument as the value for a specific parameter.

``` Python
def my_function(param_1, param_2):
    # do stuff

my_function(param_2=5, param_1=3)
```

Keyword and positional invocation can be combined, but all positional arguments must precede the keyword arguments.

Passing the same argument multiple times via keyword invocation results in a Type Error.

#### Default Arguments
Specify a default value for a parameter by defining the parameter in `<parameter>=<value>` syntax in the function definition. When a default argument is defined, the function can be invoked without providing that argument. If a value is not specified during invocation (whether positionally or by keyword), the default value will be used.

``` Python
def my_function(param_1, param_2=0):
    # do stuff

# specify only the value of the first parameter
my_function(1)
```

Parameters with default arguments must be defined after any parameters without default arguments.

Default arguments are one of the mechanisms by which Python allows for overriding functions to allow a varying signature.

#### Variable-Length Arguments - `*args`
Define a function parameter prefixed with an`*` to support accepting a variable number of arguments.

Passed arguments can then be accessed by using the specified identifier as a tuple.

``` Python
def my_func(*args):
    for arg in args:
        # do stuff
```

To unpack a collection into a variable-length parameter (instead of passing it as a single element), prefix it with `*` when invoking the function.

``` Python
my_list = [1, 5, 7]
my_func(*my_list)
```


#### Dictionary-Packed Arguments - `**kwargs`

A function parameter prefixed with `**` gets packed into a dictionary, where each keyword is used as the key; only keyword arguments will be packed.

Prefix a dictionary argument with `**` during invocation to unpack it into a series of keyword arguments (instead of passing it as a single parameter).

#### Combining Types of Arguments
The various types of arguments can be combined as long as all relevant rules are followed.
* positional arguments cannot follow keyword arguments during invocation
    - any parameters defined after a variable-length parameter can never be provided positionally during invocation
* `**` parameters must be defined after other parameter types
* only one `*` and one `**` parameter can be defined
* a `*` parameter will be unable to handle keyword arguments
* a `**` parameter will be unable to handle positional arguments


### Scoping Basics
Python follows some common scoping rules:
* functions can read and interact with variables declared globally
* variables declared locally to a function shadow globally declared variables

Python scoping / variable declaration has some quirks:
* variable declarations within a function get hoisted to the start of the function; referencing a hoisted variable before assigning a value to it results in an UnboundLocalError
* because variable declaration and assignment are the same operation in Python (`my_var = my_value`), attempting to assign a value to a global variable results in declaration of a local variable unless the variable is declared using the `global keyword`, e.g.:
    ```Python
    def my_func():
        global my_global
        # now assigning a value to my_global will modify the global variable instead of declaring a local variable
    ```

### Pass by Value vs Pass by Reference
Primitive types are passed by value by default, while reference types are passed by reference by default.

### Lambdas
Use the `lambda` kewyord to define a lambda function; its output must be assigned to a variable or passed to a function (otherwise it doesn't go anywhere). E.g.:

```Python
my_lambda = lambda x: x + 1
```

Python lambdas are only allowed to have expressions (variables and operators), no statements such as control structures (`if`, `for`, `while`, `assert`). Parameter(s) are specified left of the colon, and the result of the right-hand side is what will be returned.

Lambdas support keyword arguments, default values, `*` arguments, and `**` arguments.

Lambdas can call functions.

### Generator Functions
A generator is a function that can be used to define an iterable sequence. Generator functions use the `yield` keyword, and return a generator object when executed.

Each time `next()` is called on the generator object, the function's execution resumes after the last visited `yield` statement and proceeds to the next `yield` statement, whose value is used as the output of next.

``` Python
def gen():
    yield 1
    yield 2
    yield 3
    yield 4

generator = gen()
next(gen)
# 1
next(gen)
# 2

list(gen())
[1, 2, 3, 4]
```

A generator can be used to represent an infinite sequence. Be careful not to put an infinite list into a collection; some environments (such as Jupyter Notebook) may not be able to handle this at all gracefully.


## `assert`
An `assert` statement checks if an expression evalutes to True, and throws an Error if it does not.


## Commonly Used Modules
* math
* os
* random
* datetime
