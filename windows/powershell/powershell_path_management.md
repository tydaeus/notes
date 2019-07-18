# Manipulate Paths

## PowerShell Operators

### `Join-Path`
`Join-Path $path $childPath...` will join a path with one or more child path fragments. Adding the `-Resolve` operator will either return all paths that match a wildcarded path, or will produce an error if a non-wildcard path does not exist.

### `Split-Path`
`Split-Path` allows you to slice a path into its components.

* `-Qualifier` returns the drive (or registry root for a registry path)
* `-Leaf` returns the last item (file name) with extension
* `-Parent` (default) returns the container (containing folder)
* `-IsAbsolute` returns whether the path is absolute (false if relative)
* `-Extension` return the extension of the leaf (file extension, including '.') (PS6.0+)
* `-LeafBase` returns the base of the leaf (file name without extension) (PS6.0+)
* `-NoQualifier` omit the qualifier (the drive letter)

Add the `-Resolve` parameter to get the resolved path (resolving relative or wildcarded path to absolute equivalent). Most common usage of this is `Split-Path $Path -Parent -Resolve`, which gives the absolute path of the containing folder.

### `Resolve-Path`
The `Resolve-Path` cmdlet will attempt to resolve a path string and return a `PathInfo` object representing it. Use `(Resolve-Path $Path).Path` to access the path string.

### `Test-Path`
Use the `Test-Path` cmdlet to inspect file structures. Throws an exception if the path doesn't exist.

* Check if Path Exists: `Test-Path -Path $fullPath`
* Check if Path is Valid (not necessarily existing): `Test-Path -Path $fullPath -IsValid`
* Check if Path is Dir: `Test-Path -Path $fullPath -PathType Container`
* Check if Path is File: `Test-Path -Path $fullPath -PathType leaf`

## `System.IO.Path`
You can also use the .Net class `System.IO.Path` static methods to work with paths. Common operations:

* `Combine` - Combine up to four strings, or an array, into a path, more safely than basic string concatenation - *Warning*: if provided a relative path parameter, this will resolve relative to the user's home folder instead of PWD
* `GetDirectoryName` - *Warning*: if provided a relative path parameter, this will resolve relative to the user's home folder instead of PWD
* `GetExtension`
* `GetFileName`
* `GetFullPath` - *Warning*: if provided a relative path parameter, this will resolve relative to the user's home folder instead of PWD