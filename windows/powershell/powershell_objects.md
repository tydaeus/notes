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

### Array-String Confusion and Single-Object coercion
In addition to using the index (`[]`) operator to specify array index, PowerShell also allows using it to specify characters from a string, e.g. `'myString'[1]` is 'y'. On top of this, PowerShell's loose typing may result in a single-element string array being treated as a string unless you're very careful about using explicit variable typing/casting.

Use the `.Count` property instead of the `.Length` property - `.Count` always indicates number of elements, while `.Length` will specify number of characters when used on a string.

"Wrap" a variable you intend to reference as an array to ensure index is processed array style. E.g. `@($myVar)[0]` will always refer to the first element, not the first letter, regardless of whether `$myVar` is an array or a string.

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

Returned ArrayLists are converted to fixed-size format unless the receiving variable is explicitly an ArrayList, e.g. `[System.Collections.ArrayList]$output = Do-SomethingThatReturnsAnArrayList`.

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

For more info, the [MS deep dive](https://docs.microsoft.com/en-us/powershell/scripting/learn/deep-dives/everything-about-pscustomobject) and [gngrninja](https://www.gngrninja.com/script-ninja/2016/6/18/powershell-getting-started-part-12-creating-custom-objects) provide a variety of ways to define a PowerShell custom object.

### Adding Properties
Use the `Add-Member` cmdlet with `-Type 'NoteProperty'` to add a property to a custom object, e.g:

```powershell
$obj | Add-Member -Name 'propertyName' -Type 'NoteProperty' -Value 'propertyValue'
```

### Adding Methods
Use `Add-Member` with `-Type 'ScriptMethod'` to add a scriptblock as a method to the custom object, e.g.:

``` PowerShell
$obj | Add-Member -Name 'methodName' -Type 'ScriptMethod' -Value $methodDefinitionScriptBlock
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

**Warning**: PowerShell method behavior differs significantly from PowerShell function behavior. Among other things, output is handled differently. The standard output stream can only be written to via the `return` operator (in which case an appropriate return type must be declared). The standard error stream can only be written to or redirected if the method has a `[void]` return type (including implicit `[void]`); this appears to be handled by an implicit stderr redirect resulting in any invoked functions' `Write-Error` also being redirected to `$null`. The `-ErrorVariable` common parameter does allow capturing error output in this case, if the function/cmdlet supports this parameter.

#### Dynamically Invoking Methods
The methods available on a type can be retrieved by using the type's `GetMethods()` method.

``` PowerShell
# with the type available (i.e. within the declaring file)
[MyType].GetMethods()

# from an existing object
$myObject.GetType().GetMethods()
```

If looking specifically for user-defined functions, the `DeclaringType` and `IsSpecialName` properties can be used for filtering.

Once the method's name is assigned to a variable, it can then be invoked:

``` PowerShell
# Direct invocation - best if exact signature/arguments known
$myObject.$methodName()

# Indirect invocation - best for when signature/arguments are unknown/variable, allows passing arguments as an array
$myObject.$methodName.Invoke($argArray)
```


### Constructors
Constructors are declared similar to methods, with no return type and their name being the same as the class name.

Use `[ClassName]::new(params)` or `New-Object ClassName params` to invoke the constructor with the specified params. Parenthetical invocation appears to work better for multiple arguments.

Superclass constructors can be accessed C#-style with the `base` keyword: `ClassName ($param1) : base ($param1)`.

Constructor chaining is not permitted, but can be acheived with helper methods (e.g. having the constructors operate through one or more `hidden Init` methods).

### Inheritance
Custom classes can extend other custom classes defined within the same script by using C#-like `:` inheritance: `class ChildClass : ParentClass {...`; they can also implement .Net interfaces in the same manner.

Custom interfaces cannot currently be defined in PowerShell.

It appears to be impossible to officially extend a custom class from outside its declaring script/module, even with access to its type.

### Publishing Custom Classes
PowerShell classes don't cooperate as one might expect with the PowerShell module system.

#### By `New-` Function
The simplest, most reliable means to provide access to a custom class seems to be to define a `New-` function to act as an accessor for the class's constructor. Doing this allows for object creation, and then all properties and methods on the created object can be used. However, this leaves the class Type only accessible within the module/script that defines it, so it cannot be used for parameter typing.

#### By Running Script
1. Define the class in a `.ps1` script
2. Add the definition script to the `ScriptsToProcess` list in the module manifest (effectively ensuring that the script gets dot-sourced into scope)

This allows the class Type to be accessible, but treats the class as being outside the module's scope (thereby only allowing it access to public members of the module).

**Warning**: I haven't figured out how to get this to work consistently for repeat imports. It appears that the Type becomes accessible after one import, and then if imported again (e.g. by invoking a script that imports the module again or a second script that imports the module) the Type becomes inaccessible. Skipping type declaration if it already exists doesn't appear to work.

#### By `using` Directive
1. Define the class in a module's `RootModule` file (as determined by its manifest)
2. Include a `using` directive in scripts/modules that need to use the class.

This allows the class Type to be accessible and treats the class as being within the module's scope.

Unfortunately, `using` only works properly with modules imported from the `PSModulePath` or with their full path, allowing for scenarios where a module is visible through `Get-Module` but not available for `using`. `using` can also be inflexible due to its required invocation at the beginning of a script.

#### By `.NewBoundScriptBlock`
Using `.NewBoundScriptBlock()` to create the object against the module's scope appears to allow using the class from within the module and providing the result to the current script.

``` PowerShell
# more readable option
& (Get-Module 'ModuleName').NewBoundScriptBlock({[c]::new()})
# more pipeable option
& (Get-Module 'ModuleName') {[c]::new()}
```



## Enums
PowerShell 5.0+ provides the ability to define basic enum types:

``` PowerShell
# declare the Enum
Enum MyEnum {
    myVal1 = 1
    myVal5 = 2
    # ...
}

# use the Enum
$myVar = [MyEnum]::myVal1

# list the names for the Enum
[enum]::GetValues([MyEnum])
#>myVal1
#>myVal5

# get the numeric value for an enum value
[MyEnum]::myVal5.value__
#> 2
```