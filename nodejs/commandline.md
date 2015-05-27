# Working With the Commandline from NodeJS

### Commandline Arguments
Commandline arguments are available on the global `process` object's `argv` array. The first two arguments will be the node invocation and the script's name.

```
var args = process.argv.slice(2);
```