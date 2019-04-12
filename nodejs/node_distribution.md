# Distributing Node App
If you want to bundle a node app into an executable file, you can use one of several node utilities. A decent guide for using the `pkg` utility is at https://dev.to/jochemstoel/bundle-your-node-app-to-a-single-executable-for-windows-linux-and-osx-2c89; these notes summarize this process.

1. Install `pkg` globally via `npm install pkg -g`
2. By convention, add properties to your package.json:
    - `"main" : "main.js"` - referencing the main logic
    - `"bin" : "bin.js"` - referencing bootstrapping logic, e.g. processing arguments, requiring main, running main function
3. Run `pkg` on the app directory; use `pkg -t host` to target your current host. Your executable(s) will be generated within the current directory.
