# Custom Tasks

## Registering Task Combinations
Plugins generally provide some form of task associated with their name, which you can run by providing the name as an argument to the grunt command (`grunt uglify`). But you probably want to combine tasks, or do something of your own.
```js
// the default task is run if no argument is specified
grunt.registerTask('default',
// specifying an array of task names will run them in order
    ['uglify']);
```

## Registering Functions as Tasks

### "Basic Tasks"
You can register a function as a "basic" task via `grunt.registerTask(taskName, description, taskFunction)`:
```js
  grunt.registerTask('default', 'Log some stuff.', function() {
    grunt.log.write('Logging some stuff...').ok();
  });
```

Basic tasks are run without considering configuration or environment. Colon-separated arguments are passed, in order, as the arguments to the function.

```js
grunt.registerTask('foo', 'My "foo" task.', function(a, b) {
  grunt.log.writeln(this.name, a, b);
});

// Usage:
// grunt foo
//   logs: "foo", undefined, undefined
// grunt foo:bar
//   logs: "foo", "bar", undefined
// grunt foo:bar:baz
//   logs: "foo", "bar", "baz"
```

### "Multi" Tasks

Most plugin tasks are created as "Multi Tasks". Custom Multi Tasks can also be registered via `grunt.registerMultiTask`.

Unlike plain tasks, multi tasks:

* Can have multiple "targets", as defined under the configuration object with their name
* Run all their targets if called by name, without a specified target
* Receive access to their portion of the configuration object (the property with the same name as the task or task + target) as the `this` object

E.g.:
```js
grunt.initConfig({
  // defines configuration for task named "log"
  log: {
    // define configuration for log's targets
    foo: [1, 2, 3],
    bar: 'hello world',
    baz: false
  }
});

grunt.registerMultiTask('log', 'Log stuff.', function() {
  grunt.log.writeln(this.target + ': ' + this.data);
});
```

Given the specified configuration, this example multi task would log `foo: 1,2,3` if Grunt was run via `grunt log:foo`, or it would log `bar: hello world` if Grunt was run via `grunt log:bar`. If Grunt was run as `grunt log` however, it would log `foo: 1,2,3` then `bar: hello world` then `baz: false`.

#### Files
Files specified using the standard grunt file options are available on the `this.files` property, normalized to the Files Array format. Note that this may result in nonexistent files, so you should probably test for existence first.

## Grunt Functions
Grunt provides a number of useful functions for use in custom tasks.

### `grunt.file`
`grunt.file` provides functions for file manipulation
* `grunt.file.exists(filepath)` returns whether the file exists
* `grunt.file.read(filepath)` returns the file's content
* `grunt.file.write(path, content)` writes content to location on path

## Loading Custom Tasks
Custom tasks don't need to be in the Gruntfile; they can be loaded from an external .js file via `grunt.loadTasks()` specifying the path to the directory containing the tasks.