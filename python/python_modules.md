# Python Modules

Python programs can be organized into modules, allowing for grouping of related code. Quick reference derived from https://docs.python.org/3/tutorial/modules.html.

## Importing Modules
Use the `import` keyword to import a module, e.g. `import my_module`. Once you've imported a module, you can access symbols defined within it via the module's name and the `.` operator, e.g. `my_module.foo`.

Use the `as` keyword to designate an alternative name for accessing the module, e.g. `import my_module as mm` would allow symbol access from `mm`, e.g. `mm.foo`. This can help with brevity, or if multiple modules have the same name.

Make module members directly accessible (without the module prefix) within a script by using the `from` `import` variant, e.g. `from my_module import foo, bar`. Import `*` from a module in this way to bring all members not prefixed with `_` into the script; this is generally bad practice for script files.

Note that a given module will only be imported once within a given session, so you will need to restart the interpreter or use `importlib.reload(<modulename>)` if you wish to reload the module (i.e. to include new changes).

Use `dir()` to list currently defined names, or `dir(<modulename>)` to list names defined within the specified module.

### Module Search Order
Python variable `sys.path` lists the directories in the order they will be searched for `import` commands. `sys.path` is initialized to include:

1. The parent directory of the input script (current directory for interactive session)
2. Environment variable `PYTHONPATH`
3. Installation-dependent default if `PYTHONPATH` is not defined.

Note that this can result in custom libraries being loaded in place of standard libraries if they share a name.

`sys.path` can be modified by Python scripts.



## Writing Modules
Any normal `.py` file can be used as a module; the module name will be the name of the file without the extension.

Module statements will be run once, at the time the module is first imported, allowing for initialization, class and function declarations, etc.

Within the module, automatic variable `__name__` will evaluate to the module's name, or to special value `'__main__'` if the module is run as a script. This can be used to allow a module to act as either a library or as a script, depending on whether it has been imported or invoked.