# Python Command Line Interface (cli)

## Processing CLI Arguments
Use `sys.argv` to access the list of arguments passed on invocation of a Python script. 

The first (index 0) item in the list will be the name of the script (whether fullpath depends on OS), `-c` if Python interpreter was invoked with this option, null otherwise.

Check `if __name__ == "__main__"` to determine whether the script was invoked as the main script (otherwise it probably shouldn't be doing `argv` processing).

``` python
import sys

if __name__ == "__main__":
    print(f"Arg count: {len(sys.argv)}")
    for i, arg in enumerate(sys.argv):
        print(f"arg {i} = '{arg}'")

    scriptname = sys.argv[0]
    scriptargs = sys.argv[1:]
```

Note: `sys.argv` is a mutable global variable, so be careful of how you treat it. Consider copying or slicing a copy from it.


### `getopt.getopt`
The built-in `getopt.getopt` function parses the argument list, expecting a linux-style mix of short and long options, and possibly a list of arguments, returning the options and arguments as a tuple.

``` python
options, arguments = getopt.getopt(
    # provide the subset of argv to process
    sys.argv[1:],
    # define short options; ':' indicates option-argument expected
    'hvf:',
    # define long options; '=' indicates option-argument expected
    ["help", "version", "file="])

# each option will be a tuple of the option and (if present) its option-argument
for opt, arg in options:
    if opt in ("-h", "--help"):
        display_usage()
        sys.exit()
    if opt in ("-v", "--version"):
        display_version()
        sys.exit()
    if opt in ("-f", "--file"):
        filepath = arg
# `arguments` will be a list of the arguments passed without corresponding options

```


### `argparse.ArgumentParser`
The built-in `argparse.ArgumentParser` class allows you to build an ArgumentParser to define basic argument parsing.

``` Python
# specify usageMessage and description during construction
parser = argparse.ArgumentParser(
    usage=usageMessage, # consider including '%(prog)s' to indicate the program name
    description=scriptDescription,
)
# use add_argument method to add each argument; good to include standard version info string
parser.add_argument(
    "-v", "--version", action="version",
    version=f"{parser.prog} version 1.0.0"
)
# nargs='*' allows for variable number of items
parser.add_argument("items", nargs="*")

# call parse_args to get a representation of the arguments
args = parser.parse_args()

# found arguments end up as properties on the returned object
if args.items:
    ...

```
