# Working with Objects in PowerShell

## Arrays
Array data elements can be any type, unless array data type is declared (forcing strong typing).

### Declaring an Array
Assign a comma-separated list to a var to create an implicit array: `$arr = 1, "x", 4.6`.

Use explicit array cast syntax for legiblity, or to enforce that array creation occurs: `$arr = @("x")`.

Assign a range: `$arr = (1..4)`.

Strongly typed: `[int[]]$arr = 1, 2, 3, 5, 8`.

Specific size: `$arr = New-Object int[] $size` or `$arr = [int[]]::new($size)`

Multi-dimensional array:

```powershell
$arr = @(
    (1,3,5)
    (2,4,6)
)
```

### Add values
Append to an array using `+=`. `$arr += 8`. Note that PowerShell (stupidly, IMHO) uses immutably sized arrays, so you will be creating a new array containing all previous elements plus the new one. This can also result in referencing issues.

#### Using ArrayLists
For large mutable arrays, consider using a .Net ArrayList instead:

```powershell
# create using New-Object:
$list = New-Object System.Collections.ArrayList

# create using new method:
$list = [System.Collections.ArrayList]::new()

# Add via the add method; note that ArrayList returns the index of the added element, so we should probably pipe it to Out-Null to prevent using it as an output
$list.Add(5) | Out-Null

# Do NOT add via the += operator. This creates a new ArrayList and assigns it the original contents plus the new contents. This is inefficient and can result in reference issues.
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

```powershell
@(1, 2, 3, 4, 5) -gt 3
4
5
```

Note that this means `$null` must be on the left-hand-side if you intend to check if the array is null. Otherwise `$null` will always be returned (because `$null` is what you'll get as a result if no elements match).


## Hashtable
Create a hashtable: `[hashtable]$ht = @{}`.

Add values: `$ht.Add('PropertyName', $propertyValue)`.

Combine declaration with adding values, and use implicit typing:
```powershell
$ht = @{
    name = "foo"
    value = 5
}
```
Note that semicolons can be used in place of newlines.

By default, a hashtable won't preserve the order properties were added in. An ordered hashtable can be created in PowerShell 3.0+ by using the `[ordered]` adapter before the assignment:

```powershell
$ht = [ordered]@{
    name = "foo"
    value = 5
}
```

Access values via `$ht.name`, or `$ht.values` for the list of values. Use `$ht.keys` to get a list of the keys present in the HashTable.

Note that the `foreach` loop does not include direct support for HashTables, so you'll probably want to iterate through the keys, or use the `GetEnumerator` method.

### "Splatting" a Hashtable
Referencing a hashtable variable with the `@` operator will convert it into a PowerShell parameter string.

## Custom Objects
In PowerShell 3.0+, the `[PSCustomObject]` type adapter is the easiest way to quickly create an object:

```powershell
$obj = [PSCustomObject]@{
    name = "foo"
    value = 5
}
```

Properties can be added subsequently via the `Add-Member` cmdlet: `$obj | Add-Member -MemberType NoteProperty -Name name -Value "foo"`.

For more info, [gngrninja](https://www.gngrninja.com/script-ninja/2016/6/18/powershell-getting-started-part-12-creating-custom-objects) provides a variety of ways to define a PowerShell custom object.

### Adding Properties
Pipe a custom object into the `Add-Member` cmdlet to add a property to it, e.g:

```powershell
$obj | Add-Member -Name "propertyName" -Type NoteProperty -Value "propertyValue"
```

## Custom Classes
Courtesy of https://4sysops.com/archives/powershell-classes-part-1-objects/.

Declare a class using the `Class` command:

```powershell
Class Foo {
    [type]$PropName = $DefaultValue
    #Note: no comma-separation between properties, must be line/semicolon separated
    #...
}
```

Instantiate with either `$instance = [Foo]::new()` or `$instance = New-Object Foo`.

Access a property using the `.` operator: `$instance.PropName`. Note that the `$` symbol gets dropped from the property name.

### Property Modifiers

#### `hidden`
Declare a property with the `hidden` modifier to prevent it from displaying in the PowerShell console by default. Use `Get-Member`'s  `-Force` parameter to display hidden properties.

```powershell
Class Foo {
    hidden [type] $PropName = $DefaultValue
}
```

#### `static`
`Static` properties have a value at the class-level, instead of the instance level.

```powershell
Class Foo {
    static [type] $StaticProp = $DefaultValue
}
```

Access `static` properties using the `::` operator on the class `[Foo]::StaticProp` or an object `$instance::StaticProp`.

### Methods
Declare a method on a class using:

```powershell
#...
    [ReturnType]MethodName([Type]Parameter1, [Type]Parameter2) {
        #...
    }
#...
```

Use the `$this` variable to reference the calling instance.

Use the `static` modifier to declare a method as static to the class.

PowerShell methods can be overloaded by declaring additional methods of the same name, with a different signature.

### Constructors
Constructors are declared similar to methods, with no return type and their name being the same as the class name.

Use `[ClassName]::new(params)` or `New-Object ClassName params` to invoke the constructor with the specified params. Parenthetical invocation appears to work better for multiple arguments.
