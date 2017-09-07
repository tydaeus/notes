# File Operations in C\#

File operations in C\# are accomplished via System.IO.

## Exists

Use `File.Exists(path)` to check if a (non-directory) file exists.
Use `Directory.Exists(path)` to check if a directory exists.

## Path utilities

`System.IO.Path` provides utilities for manipulating filepaths. In particular, `Path.Combine` allows for string concatenation with *some* awareness of proper filepath structure (See official docs for specifics).