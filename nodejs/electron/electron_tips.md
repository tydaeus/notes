# Electron Tips and Tricks

## Initialization
Electron apps are initialized by running electron on the "main" js file. This can be accomplished by:

* Installing the electron global app (download from the electron site) and using that: `electron main.js`
* Launching the electron app from the `node_modules` folder
	- `./node_modules/.bin/electron main.js`
	- `node ./node_modules/electron/electron.js main.js`

## Accessing electron runtime
Because application pages are handled as web pages, they don't get direct access to the electron runtime (and thus to the node js runtime).

To access the electron runtime, you need to `require` the `electron` module, via: `require('electron')`. The attached `remote` property appears to provide access to much of the node environment, including the `process` object.

Properties declared on the `global` object in `main.js` can be accessed via `remote.getGlobal(objectName)`, e.g.:

```
// main.js
global.dataObject = {prop1: 'x'};

// caller.js
const dataObject = require('electron').remote.getGlobal('dataObject');
```

### Learning - How to `require`?
In my early experiments I've used Browserify to ensure that `require` is available. However, it appears that this clobbers a `require` global function that is built-in as part of running in the electron environment.

### Issue - `require` conflict with Browserify
Because Browserify clobbers the `require` global function injected by electron, `require('electron')` fails when running an app that has been packed by Browserify (other modules may also fail). To work around this, you can use `window.require('electron')` instead.

## Simple Feature Toggling
Use the process argument array to enable some simple feature toggling.

```
const argv = require('electron').remote.process.argv
```

Or you can use the global object (as mentioned in "Accessing electron runtime") to provide a more processed version of this via `remote.getGlobal`.
