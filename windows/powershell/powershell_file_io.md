# PowerShell File IO

## Write a File

### Encoding
For Microsoft-known reasons, PowerShell output and redirection default to UTF-16 encoding with a Byte-Order Mark. This can be problematic.

This default can be changed by setting `$PSDefaultParameterValues['Out-File:Encoding'] = 'utf8'`. In PowerShell 3.0+, this modifies behavior of Out-File only. In PowerShell 5.1+, this also modifies the behavior of the redirect operators.

### Redirection
Redirection can be used to write to files: `$myString > $filePath`. Note, however, that this encodes the file as UTF-16 by default.

Redirection can also be used to append to files: `$myString >> $filePath`.

### Out-File
The `Out-File` cmdlet allows writing to files with some options. `$myString | Out-File $MyPath`.

This can be used to change encoding to UTF-8: `$myString | Out-File -Encoding "UTF8" $MyPath`. However, the Byte-Order Mark (`ï»¿`) will be written as the first several characters, which may result in incompatibility.

### Set-Content and Add-Content
The `Set-Content` cmdlet replaces the content of one or more files with specified content, e.g. `Set-Content $Path $Value`.

The `Add-Content` cmdlet appends content to one or more files.

This appears to default to UTF8 encoding without a Byte-Order Mark.

### `File::WriteAllLines`
Use .NET's `File` class static `WriteAllLines` method: `[System.IO.File]::WriteAllLines($MyPath, $MyString)`. In PowerShell 4.0+, this will default to UTF 8 with no Byte-Order Mark.

In PowerShell 3.0, use `[System.IO.File]::WriteAllLines($MyPath, $MyString, [System.Text.UTF8Encoding]::new($False))` to explicitly indicate UTF-8 with no BOM.

## Read a File

### Line-by-Line: `Get-Content`
Use `Get-Content $filePath` to retrieve a file's content as a collection of lines. Pipe this into `ForEach-Object` or use in a `foreach` loop to iterate through the lines.

`Get-Content` has known performance issues on large files, because it must read through the entire file before returning.

### Line-by-Line: `System.IO.File::ReadLines`
Use `[System.IO.File]::ReadLines($filePath)` to access .Net's `File.ReadLines` function to retrieve a file's content line by line. Pipe this into `ForEach-Object` or use in a `foreach` loop to iterate through the lines.
