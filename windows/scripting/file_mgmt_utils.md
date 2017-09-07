# Windows Command-line File Management Utilities
The windows command line file management utilities are a fragmented and confusing ecosystem.

## Copying Files

### Copy
`copy` provides basic file copying utility. It is a very primitive tool, so `xcopy` or `robocopy` may be preferrable. However, there are some operations that can be performed most easily with `copy`.

#### Updating Modified Time (Touch)
`copy` can be used to update the modified time of a given file, by writing the file over itself.

In current working directory:
```cmd
copy /b FILENAME+
```

In another directory:
```cmd
copy /b PATH\FILENAME+,, PATH\
```

### XCopy
`xcopy` copies files and/or directory trees, providing more options than `copy`. `xcopy` has been **deprecated** after Vista/Windows 2008, so use `robocopy` instead, if available.

```cmd
xcopy src dest
```

#### Common Flags

* `/E` - copy folders and subfolders, including empty folders
* `/Y` - do not prompt before overwriting
* `/-Y` - prompt before overwriting (default)
* `/Q` - quiet mode (do not display file names while copying)

#### File Type Specification
By default, `xcopy` prompts the user to specify whether files are regular files or directories. Pipe `echo` with 'D' or 'F' to specify ahead of time.

```cmd
echo F | xcopy src.txt dest.txt
```

## Directories

## `md` - Make Directory
The `md` command creates a new directory.

## `rd` - Remove Directory
The `rd` command removes a directory. Without options, this will require confirmation, and will not remove non-empty directories.

### Common Flags

* `/S` - delete all files and subfolders (removes an entire file tree)
* `/Q` - do not require confirmation; this will also cause silent failure if the directory is not empty and `/S` is not specified


### Typical Usage
```cmd
rd /S /SQ mydir
```

