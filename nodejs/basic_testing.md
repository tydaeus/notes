# Basic NodeJS Testing

## `http-server` Static File Server
(notes copied from the npm listing)

The `http-server` node utility can be used to quickly setup a server for 
testing that your static files work correctly.

### Install
Install with `npm install -g http-server`.

### Usage

```
http-server [path] [options]
```

`[path]` defaults to `./public` if the folder exists, and `./` otherwise.

Now you can visit [http://localhost:8080] to view your server