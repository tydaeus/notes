# Working With the Commandline from NodeJS

### Commandline Arguments
Commandline arguments are available on the global `process` object's `argv` array. The first two arguments will be the node invocation and the script's name.

```
var args = process.argv.slice(2);
```

### Commandline Prompt Input
Input can be solicited from the commandline by processing the raw streams provided in `process.stdin` and `process.stdout`, through the `readline` module, or the third-party `prompt` module.

Readline example tinyPrompt (from the nodejs docs):
```javascript
var readline = require('readline'),
    rl = readline.createInterface(process.stdin, process.stdout);

rl.setPrompt('OHAI> ');
rl.prompt();

rl.on('line', function(line) {
  switch(line.trim()) {
    case 'hello':
      console.log('world!');
      break;
    default:
      console.log('Say what? I might have heard `' + line.trim() + '`');
      break;
  }
  rl.prompt();
}).on('close', function() {
  console.log('Have a great day!');
  process.exit(0);
});
```
* `rl.close()` will close the prompt to input