# Common PowerShell Tasks

## Read a File

### Line-by-Line: `Get-Content`
Use `Get-Content $filePath` to retrieve a file's content line by line. Pipe this into `ForEach-Object` or use in a `foreach` loop to iterate through the lines.

`Get-Content` has known performance issues on large files.

### Line-by-Line: `System.IO.File::ReadLines`
Use `[System.IO.File]::ReadLines($filePath)` to access .Net's `File.ReadLines` function to retrieve a file's content line by line. Pipe this into `ForEach-Object` or use in a `foreach` loop to iterate through the lines.

### XML Parsing
Retrieve an xml file via `[xml]$xmlFile = Get-Content $xmlFilePath`. "Dot-Walk" on the object to retrieve the contents of all tags of a specified nesting and type. E.g. `$xmlFile.xml.TagLev1.TagLev2` will retrieve the contents of all TagLev2 tags that are contained within TagLev1 tags which are contained within an xml tag. Note that not all xml files use the `xml` tag as a base-level tag.


## Get Contents of a Directory
`Get-ChildItem $dirPath`. Use the `-Recurse` parameter for recursive traversal, and the `-Force` parameter to include hidden and system files.

### Traverse Directory Recursively

#### Piping and Filtering to get Specific FileNames
`Get-ChildItem $dirPath -Recurse` for all files.

`Get-ChildItem $dirPath -Recurse | Where-Object {$_.Extension -eq $ext}` to include only the specified extension.

`Get-ChildItem $dirPath -Recurse | Where-Object {$_.Extension -eq $ext} | select FullName` to convert to a list of all matching files, with their full paths.

This pattern can be modified to select based on other features of the file, too.

#### Via Custom Function, Allowing Advanced Processing
This option can be adapted to exclude some directories from the recursion. Adapted from https://stackoverflow.com/a/10585857/2939139:

```
function GetFiles($path = $pwd)
{
    foreach ($item in Get-ChildItem $path)
    {
        # return the item; substitute with other processing, if desired
        $item

        if (Test-Path $item.FullName -PathType Container)
        {
            GetFiles $item.FullName
        }
    }
}
```

See the original answer for a more elaborate solution that supports piping.

## Check on Files at Path
Use the `Test-Path` cmdlet to inspect file structures. Use `Get-Help Test-Path` for full help.

### Check if Path Exists
`Test-Path -Path $fullPath`

### Check if Path is Valid
`Test-Path -Path $fullPath -IsValid`

### Check if Path is Dir
`Test-Path -Path $fullPath -PathType Container`

### Check if Path is File
`Test-Path -Path $fullPath -PathType leaf`
