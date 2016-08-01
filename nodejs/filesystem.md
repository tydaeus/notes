#Nodejs Filesystem

From https://nodejs.org/api/fs.html:
>File I/O is provided by simple wrappers around standard POSIX functions. To use this module do require('fs'). All the methods have asynchronous and synchronous forms.
  
>The asynchronous form always take a completion callback as its last argument. The arguments passed to the completion callback depend on the method, but the first argument is always reserved for an exception. If the operation was completed successfully, then the first argument will be null or undefined.

>When using the synchronous form any exceptions are immediately thrown. You can use try/catch to handle exceptions or allow them to bubble up.

### Listing files in a directory

```js
fs.readdir(dirPath, function callback(err, files) { ... });

var files = fs.readdirSync(dirPath);

// where files is an array of the filenames, excluding . and ..

```


### Getting information about a file

```js
fs.stat(path, function callback(err, stats) {
    stats.isFile();
    stats.isDirectory();    
});

```
Note that `stat` results in a 
fs.statSync is the synchronous version.

### Reading data from a file

```js
fs.readFile(filePath, [options], callback);
// filePath is a string representing the path to the file
// encoding is one of "ascii", "utf8", or "base64"
// callback is called once contents are ready, with signature (err, data)
```
* `filePath` string representing path to file
* `options` Object
    - `encoding` String | Null (default = null)
        + if not specified / null, raw buffer is returned
        + can be used in place of options object
    - `flag` String (default = "r")
* `callback` Function (err, data);

`fs.readFileSync(filename[, options])` provides the synchronous equivalent, returning the file's contents.

### Writing data to a file

`fs.writeFile(filename, data[, options], callback)`
* `filename` String representing path to file
* `data` String | Buffer
* `options` Object
    - `encoding` String | Null default = "utf8"
    - `Mode` Number default = 438 (Octal 0666)
    - `flag` String default = "w"
* `callback` Function (err)

`fs.writeFileSync(filename, data[, options])` provides synchronous equivlant, no return.

### Deleting Files

* `fs.rmdir(dirname, callback)` or `fs.rmdirSync(dirname)` can be used to delete an empty directory
* `fs.unlink(filename, callback)` or `fs.unlinkSync(filename)` can be used to delete a file

```javascript
    // deletes a file and all contents within if directory
    function rmRecursive(file) {

        if (fs.statSync(file).isDirectory()) {

            var contents = fs.readdirSync(file);

            contents.forEach(function (item) {
                rmRecursive(file + "/" + item);
            });

            fs.rmdirSync(file);

        } else {
            fs.unlinkSync(file);
        }
    }
```

### Creating a Directory
`fs.mkdir(path, [mode], callback)`