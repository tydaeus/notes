# Distributing Node App
If you want to bundle a node app into an executable file, you can use one of several node utilities. A decent guide for using the `pkg` utility is at https://dev.to/jochemstoel/bundle-your-node-app-to-a-single-executable-for-windows-linux-and-osx-2c89; these notes summarize this process. See https://www.npmjs.com/package/pkg for documentation on `pkg`.

## Installation
Install `pkg` globally via `npm -g install pkg`

## Invocation
Run one of:
    - `pkg {PATH}/main.js` - main.js will be used as the entry point on invocation.
        - **Recommended**: ensure the `bin` property in `package.json` also references your entry point
    - `pkg {PATH}/package.json` - property `bin` defined within `package.json` will be used to determine entry point
    - `pkg {PATH}` - `package.json` on `{PATH}` will be used as per the above invocation

## Output Targetting
By default executables will be created for linux, macos, and windows, targeting the current architecture and node version. Use `-t host` to target only the running platform, architecture, and node version.

## Asset Dependencies
Any non-js file dependencies should be referenced relative to the `__dirname` constant.

### Asset Detection
If assets are referenced within the context of `path.join(__dirname, 'path/to/asset')`, where `path.join` has exactly two arguments, the first being `__dirname` and the second being a string literal, `pkg` will detect the dependency and include it.

### Asset Declaration
If assets are not referenced as described under "Asset Detection", they must be declared within `package.json` as a string or array of strings under `pkg.assets`. `pkg` must also be invoked in one of the formats that utilizes `package.json` in order to include these assets.

## Troubleshooting

### `Error! This experimental syntax requires enabling one of the following parser plugin(s): 'decorators-legacy, decorators'`
`pkg` relies on the `babel` parsing library, and assumes that all content within `node_modules` needs to be parsed. `.css` files (and possibly others) will generate errors as a result. Remove css from packages, reference it from other folders if needed.

### Unable to Download Binaries When Running `pkg`
`pkg` uses the `pkg-fetch` project to retrieve needed node binaries. Download the binaries you need from `https://github.com/zeit/pkg-fetch/releases` and place within the `.pkg-cache` folder (by default located within your user home folder, override by setting the `PKG_CACHE_PATH` environment variable.
