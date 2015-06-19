# Node global and injected local vars

* `__dirname` is the name (absolute path) of the directory containing the current module
* `__filename` is the name (absolute path) of the file containing the current module
**Note:** on Windows, backslash will be used as the file separator within these paths, but a forward slash can be used instead. Consider pre-processing to replace all backslashes with forward slashing if the application will need to run under both Windows and Linux.
```javascript
__dirname = __dirname.replace(/\\/g, "/");
```


## Process object
The `process` object provides access to a number of methods and variables pertaining to the running process
* `process.exit(status)` causes the current process to exit with the provided status code
* `process.argv` is the array containing the commandline arguments this process was started with
    - `process.argv[0]` will be "node"
    - `process.argv[1]` will be the name of the script being run

