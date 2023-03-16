# Python Command Line Interface (cli)

## Processing CLI Arguments
Use `sys.argv` to access the array of arguments passed on invocation of a Python script.

Check `if __name__ == "__main__"` to determine whether the script was invoked as the main script (otherwise it probably shouldn't be doing `argv` processing).

``` python
import sys

if __name__ == "__main__":
    print(f"Arg count: {len(sys.argv)}")
    for i, arg in enumerate(sys.argv):
        print(f"arg {i} = '{arg}'")
```