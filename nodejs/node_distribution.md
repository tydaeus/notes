# Distributing Node App
If you want to bundle a node app into an executable file, you can use one of several node utilities. A decent guide for using the `pkg` utility is at https://dev.to/jochemstoel/bundle-your-node-app-to-a-single-executable-for-windows-linux-and-osx-2c89; these notes summarize this process. See https://www.npmjs.com/package/pkg for documentation on `pkg`.

1. Install `pkg` globally via `npm install pkg -g`
2. Run one of:
    - `pkg {PATH}/main.js` - main.js will be used as the entry point on invocation
    - `pkg {PATH}/package.json` - property `bin` defined within `package.json` will be used to determine entry point
    - `pkg {PATH}` - `package.json` on `{PATH}` will be used as per the above invocation

By default executables will be created for linux, macos, and windows, targeting the current architecture and node version. Use `-t host` to target only the running platform, architecture, and node version.
