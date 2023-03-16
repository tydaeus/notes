# Python Command Line Interface (cli)

## Processing CLI Arguments
Use `sys.argv` to access the array of arguments passed on invocation of a Python script.

Check `if __name__ == "__main__"` to determine whether the script was invoked as the main script (otherwise it probably shouldn't be doing `argv` processing).