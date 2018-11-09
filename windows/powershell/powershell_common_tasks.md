# Common PowerShell Tasks

## Get Contents of a Directory
`Get-ChildItem $dirPath`

### Traverse Directory Recursively
Adapted from https://stackoverflow.com/a/10585857/2939139:

```
function GetFiles($path = $pwd)
{
    foreach ($item in Get-ChildItem $path)
    {
        # return the item
        $item

        if (Test-Path $item.FullName -PathType Container)
        {
            GetFiles $item.FullName $exclude
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
