# Getting Started with Grunt
Notes adapted from the official grunt documentation at http://gruntjs.com

## Installation
Grunt requires npm.
```sh
npm install -g grunt-cli
```

## Setting up a Project
You'll need the `package.json` file created by npm. Then you can either manually create a `Gruntfile.js` (or `Gruntfile.coffee`), or use the `grunt-init` utility (available separately from npm). `grunt-init` uses templates installed in the `~/.grunt-init directory`.

### The Gruntfile
The Gruntfile specifies initializing and configuration for running grunt. This is a nodejs file that exports the *wrapper function* documented below, and performs grunt-specific tasks initialized by this wrapper function.

#### Wrapper Function
Grunt utilizes the Gruntfile by assuming that it follows a structure that exports a single function, which takes a "grunt" object as its only parameter:
```js
module.exports = function(grunt) {
    // do grunt stuff
};
```
The Gruntfile uses methods on this object to influence the way grunt behaves.

#### Configuration
Use the `grunt.initConfig()` method to specify configuration for the grunt run.
```js
grunt.initConfig({
    // arbitrary data can be added to the configuration; here we read in the
    // package.json file and make it available as the pkg object in the config
    pkg: grunt.file.readJSON('package.json'),
    // most grunt plugins (such as uglify) expect their configuration to be 
    // specified on the config as a property with their name
    uglify: {
        options: {
            // <% %> template strings can reference config properties or invoke
            // grunt object methods
            banner: '/*! <%= pkg.name %> <%= grunt.template.today("yyyy-mm-dd") %> */\n'
        },
        build: {
            src: 'src/<%= pkg.name %>.js',
            dest: 'build/<%= pkg.name %>.min.js'
        }
    }
});
```
Most plugins allow specifying multiple configurations, by specifying a "main"

#### Installing Grunt Plugins
Install grunt plugins using npm:
```sh
npm install GRUNT-MODULE --save-dev
```

#### Loading plugins and tasks
Any plugins specified in package.json and installed via npm install can be loaded with `grunt.loadNpmTasks()`.
```js
grunt.loadNpmTasks('grunt-contrib-uglify');
```
List available tasks with `grunt --help`.

#### Custom Tasks
Plugins generally provide some form of task associated with their name, which you can run by providing the name as an argument to the grunt command (`grunt uglify`). But you probably want to combine tasks, or do something of your own.
```js
// the default task is run if no argument is specified
grunt.registerTask('default',
// specifying an array of task names will run them in order
    ['uglify']);
```

You can also register your own custom tasks by writing js functions; see [Custom Tasks](custom_tasks.md) for more info. 

## Running Tasks
Once a given task has been loaded and or registered, it can be run from the command line as an argument to the `grunt` command.

```sh
# no arguments will run the task registered as "default"
grunt 
# will run the task with name "taskName"
grunt taskName
# will run the target "taskTarget" within task "taskName"
grunt taskName:taskTarget
```



