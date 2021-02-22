# PowerShell with Excel

PowerShell can be used to read and modify Excel files. Based on https://mcpmag.com/articles/2018/04/06/find-excel-data-with-powershell.aspx, and https://community.idera.com/database-tools/powershell/powertips/b/tips/posts/changing-excel-cells-from-powershell, plus experimentation.

## Opening Excel Worksheet

```PowerShell
$Excel = New-Object -ComObject Excel.Application
# Optional: set the Excel instance to be visible
$excel.Visible = $true


# This must be the absolute path to the workbook, because COM objects don't operate relative to PowerShell working dir
$workbook = $Excel.Workbooks.Open("$WorkBookFullPath")
# Access worksheet by index; all Excel (and Office?) objects are 1-indexed
$workSheet = $workbook.Sheets.Item(1)
# Verify worksheet name:
$workSheet.Name
```

```PowerShell
# Access worksheet by name; throws System.Runtime.InteropServices.COMException indicating invalid index if no sheet found
$workSheet = $workbook.Sheets($sheetName)
```

## Locating Data

```PowerShell
# Read the text in cell A2; note how cells are specified in Row,Column order
$workSheet.Cells(1,2).Text

# Find the specified search text, capturing the search results in $Found
# Searches can use wild cards: '*' for multiple characters, '?' for single-character
$found = $workSheet.Cells.Find($searchText)
# capture the address of the first item found, because searching loops
$startingAddress = $Found.Address() # Note: there are a variety of formats for address; Address(0,0,1,1) is more absolute

Do {
    # Access properties of the current search result from $Found
    $found.Text

    # go to the next search result
    $found = $WorkSheet.Cells.FindNext($Found)
    $curAddress = $found.Address()
}
While ($startingAddress -ne $curAddress)
```

## Getting a Range

```PowerShell
# Get a reference to a range; unlike `Cells() or `Cells.Item()` this method supports the various Excel addressing methods
$range = $WorkSheet.Range('A15:B20')

# Address cells within the range relative to its top left; this is cell A15
$range.Cells(1,1)

# Note that you can also reference cells outside the range relative to the range (but negative indexes are not permitted)
$range.Cells(100,1)

```

## Modifying Data

```PowerShell
$WorkSheet.Cells.Item($row,$column) = $RevisedValue
```

## Saving Data
Use `$Workbook.Save()` or `$Workbook.SaveAs($FullPath)` to save the modified file.

## Closing the Workbook
Use `$Workbook.Close()` to close the workbook.