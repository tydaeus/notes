# Getting Started with Express

#### Prereqs
* node.js
* npm
* express-generator module should be installed globally via `$ npm install express-generator -g`

## Initial Setup
To start express project setup, navigate to the desired root folder and execute the `express` command. Common options:
* `--css less|stylus` add Less or Stylus CSS pre-processing to project
* `--ejs` use EJS HTML template engine (instead of Jade default)
* `--jshtml` use JsHtml template engine (instead of Jade default)
* `--hogan` use Hogan template engine (instead of Jade default)

The express command will create a new node.js application with a package.json file describing all dependencies needed for the selected Express installation.
These dependencies will be installed once `npm install` has been run.

```sh
$ express myapp --css less
```

## Running the Application
After creating the application, run
```sh
$ node ./bin/www
```
For development purposes, consider using `nodemon` instead of `node`. `nodemon` will automatically restart the application after changes.

## Debugging
The `debug` module allows conditional display of log-style output.
```js
var debug = require('debug')('namespace');

debug("prints formatted message", vars);
```
Set The `DEBUG` system variable to instruct express to display logging output.
* `DEBUG=express:*` - display all express internal logs
* `DEBUG=express:application` - display logs from application implementation
* `DEBUG=appname` - display debug statements from app generated via `$ express appname`
* specify multiple debug namespaces with a comma-separated list of namespaces

