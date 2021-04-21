# PowerShell File IO

Sources/References:

* https://powershellexplained.com/2017-03-18-Powershell-reading-and-saving-data-to-files/

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
These are probably the **best general-purpose** ways to modify a file.

The `Set-Content` cmdlet replaces the content of one or more files with specified content, e.g. `Set-Content $Path $Value`.

The `Add-Content` cmdlet appends content to one or more files. Note that this appears to provide little/no buffering, and so will operate very slowly if used repeatedly. Consider using .Net's `StreamWriter.WriteLine` method for better performance when writing large numbers of single lines.

This appears to default to UTF8 encoding without a Byte-Order Mark.

Note: These cmdlets apply filesystem wildcarding when interpreting their `Path` parameter; the `LiteralPath` parameter should be used if wildcard should be ignored.

### `File::WriteAllLines`
Use .NET's `File` class static `WriteAllLines` method: `[System.IO.File]::WriteAllLines($MyPath, $MyString)`. In PowerShell 4.0+, this will default to UTF 8 with no Byte-Order Mark.

In PowerShell 3.0, use `[System.IO.File]::WriteAllLines($MyPath, $MyString, [System.Text.UTF8Encoding]::new($False))` to explicitly indicate UTF-8 with no BOM.


### `StreamWriter.WriteLine`
Create a .Net `StreamWriter` and use its `WriteLine` method if you want to be able to write single lines repeatedly with reasonable performance (e.g. writing from the pipline or generating on the fly).

```PowerShell
try
{
    # Important: $outputFilePath must provide the absolute file path
    $stream = [System.IO.StreamWriter]::new($outputFilePath)

    while (<# generating lines #>) {
        # generate $line
        $stream.WriteLine( $line )
    }
}
finally
{
    $stream.close()
}
```

## Read a File

### Line-by-Line: `Get-Content`
`Get-Content` is the best file reading option for **most general cases**.

Use `Get-Content $filePath` to retrieve a file's content as a collection of lines. Pipe this into `ForEach-Object` or use in a `foreach` loop to iterate through the lines.

`Get-Content` has known performance issues on large files, because it must read through the entire file before returning.

Use the `-Tail` switch to read from the end.

Use the `-Raw` switch to get the content as a single string (useful for reading multi-line formats such as JSON).

Note: `Get-Content` applies filesystem wildcarding to its `Path` parameter; use the `LiteralPath` parameter to avoid this.

### Line-by-Line: `System.IO.File::ReadLines`
Use `[System.IO.File]::ReadLines($filePath)` to access .Net's `File.ReadLines` function to retrieve a file's content line by line, returning the lines as they're read. Pipe this into `ForEach-Object` or use in a `foreach` loop to iterate through the lines. *Note*: you will need to provide the full path to the file, because .Net calls don't operate from `$PWD`.

Example courtesy of https://stackoverflow.com/a/12876884/2939139:

```PowerShell
foreach ($line in [System.IO.File]::ReadLines($filename)) {
    # do something with $line
}
```

### Line-by-Line:
You can alternatively use .Net `fileReader.ReadLine` to read a file line by line more manually. This also allows for stopping at an arbitrary point.

Example adapted from https://stackoverflow.com/a/4192419/2939139.

``` PowerShell
$reader = [System.IO.File]::OpenText($fullInputFilePath)
try {

    $line = $reader.ReadLine()

    # ReadLine returns $Null when it reaches EOF
    while ($line -ne $Null)
        # process $line

        $line = $reader.ReadLine()
    }
}
finally {
    $reader.Close()
}
```