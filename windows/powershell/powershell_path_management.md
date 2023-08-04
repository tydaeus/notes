# Manipulate Paths

## PowerShell Cmdlets

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

`Resolve-Path` errors out by default if no matching file is found. Set `-ErrorAction` to `SilentlyContinue` if this is not desired. The error variable can be processed to retrieve what the path would resolve to if it existed (from https://stackoverflow.com/a/12605755/2939139):

```PowerShell
$FileName = Resolve-Path $FileName -ErrorAction SilentlyContinue -ErrorVariable _frperror
    if (-not($FileName)) {
        $FileName = $_frperror[0].TargetObject
    }
```

### `Test-Path`
Use the `Test-Path` cmdlet to inspect file structures. Throws an exception if the path doesn't exist.

* Check if Path Exists: `Test-Path -Path $fullPath`
* Check if Path is Valid (not necessarily existing): `Test-Path -Path $fullPath -IsValid`
* Check if Path is Dir: `Test-Path -Path $fullPath -PathType Container`
* Check if Path is File: `Test-Path -Path $fullPath -PathType leaf`

## `System.IO.Path`
You can also use the .Net class `System.IO.Path` static methods to work with paths. Common operations:

* `Combine` - Combine up to four strings, or an array, into a path, more safely than basic string concatenation
* `GetDirectoryName`
* `GetExtension`
* `GetFileName`
* `GetFileNameWithoutExtension`
* `GetFullPath`

*Warning*: the .NET runtime doesn't have access to the PowerShell PWD, so .NET methods will resolve relative to the user's home folder instead. Prefer full paths over relative paths.


## `System.Uri`
Cast the string path to `System.Uri` (e.g. `$uri = [System.Uri]$path`) to check various properties.

* `$uri.IsUnc` - whether the path is a UNC (network share) path


## Known Issue: Long Paths
PowerShell 5.1 and earlier have some issues with long filepaths.

Enable long filepath support via `Set-ItemProperty 'HKLM:\System\CurrentControlSet\Control\FileSystem' -Name 'LongPathsEnabled' -value 1`. This will automatically allow some cmdlets (e.g. `Get-ChildItem`) to work correctly with long paths.

Some cmdlets (e.g. `Remove-Item`) will require the path to be prefixed with `\\?\`.

Some cmdlets (e.g. `Expand-Archive`) do not appear to support long paths in PowerShell 5.1. Install a newer PowerShell or use other utilities (e.g. 7zip).