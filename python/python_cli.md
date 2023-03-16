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