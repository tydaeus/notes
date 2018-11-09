# Working with Objects in PowerShell

## Arrays
Array data elements can be any type, unless array data type is declared (forcing strong typing).

### Declaring an Array
Assign a comma-separated list to a var to create an implicit array: `$arr = 1, "x", 4.6`.

Use explicit array cast syntax for legiblity, or to enforce that array creation occurs: `$arr = @("x")`.

Assign a range: `$arr = (1..4)`.

Strongly typed: `[int[]]$arr = 1, 2, 3, 5, 8`.

Multi-dimensional array:

```
$arr = @(
    (1,3,5)
    (2,4,6)
)
```

### Add values
Append to an array using `+=`. `$arr += 8`. Note that PowerShell (stupidly, IMHO) uses immutably sized arrays. For large arrays, consider using a .Net ArrayList instead:

```
$list = New-Object System.Collections.ArrayList
$list.Add(5)
```

### Retrieve Items
To return all elements, just reference the array name: `$arr`.

Access a specific elements via index: `$arr[2]`.

Access a range: `$arr[2..5]`.

Use negative indexes to retrieve from the end, so `$arr[-1]` will retrieve the last element.

Populate multiple vars from array elements: `$v1,$v2 = $arr`.

Combine indices with a range, separating with a plus: `$arr[1,2+4..9]`.

Get array length: `$arr.length`.

Loop through elements: `foreach ($elem in $arr) { $elem }`.

### Comparisons
Comparison operators function as a filter, returning all matching values.

```
@(1, 2, 3, 4, 5) -gt 3
4
5
```

Note that this means `$null` must be on the left-hand-side if you intend to check if the array is null. Otherwise `$null` will always be returned (because `$null` is what you'll get as a result if no elements match).


## Hashtable
Create a hashtable: `[hashtable]$ht = @{}`.

Add values: `$ht.Add('PropertyName', $propertyValue)`.

Combine declaration with adding values, and use implicit typing:
```
$ht = @{
    name = "foo"
    value = 5
}
```
Note that semicolons can be used in place of newlines.

By default, a hashtable won't preserve the order properties were added in. An ordered hashtable can be created in PowerShell 3.0+ by using the `[ordered]` adapter before the assignment:

```
$ht = [ordered]@{
    name = "foo"
    value = 5
}
```

Access values via `$ht.name`, or `$ht.values` for the list of values.

## Custom Objects
In PowerShell 3.0+, the `[PSCustomObject]` type adapter is the easiest way to quickly create an object:

```
$obj = [PSCustomObject]@{
    name = "foo"
    value = 5
}
```

Properties can be added subsequently via the `Add-Member` cmdlet: `$obj | Add-Member -MemberType NoteProperty -Name name -Value "foo"`.
