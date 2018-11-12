# Electron Tips and Tricks

## Initialization
Electron apps are initialized by running electron on the "main" js file. This can be accomplished by:

* Installing the electron global app (download from the electron site) and using that: `electron main.js`
* Launching the electron app from the `node_modules` folder
	- `./node_modules/.bin/electron main.js`
	- `node ./node_modules/electron/electron.js main.js`

## Accessing electron runtime
Because application pages are handled as web pages 