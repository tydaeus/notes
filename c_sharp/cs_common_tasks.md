# C# common tasks

## Extend Class / Implement Interface
Use the `:` operator to inherit from a class and/or implement interfaces.

``` C#
class ChildClass : ParentClass { ... }
```

### Call Superclass Constructor
Use the `:` operator with the `base` in a constructor declaration to call the superclass's constructor:

``` C#
public ChildClass(type paramVar) : base (paramVar) { ... }
```

## Getters and Setters
C# supports property getters and setters; these operate on a private backing field. The backing field is compiler-generated if using default getter and setter definitions, but must be manually defined if using custom-defined getter/setter.

``` C#
// set and get both public
public type MyProperty { get; set; }

// public get private set
public type MyProperty { get; private set; }

// public get, no setter
public type MyProperty { get; }

// custom getter and setter require that backing field be defined manually; both must be custom if either is
// backing field by convention is named same as property with underscore prefix and first name of property
private type _myProperty;

public type MyProperty {
    get {
        return _myProperty;
    }

    set {
        _myProperty = value;
    }
}
```


## Add a NuGet Package
1. Under `Tools > NuGet Package Manager` select `Manage NuGet Packages for Solution...`
2. Browse for the desired package
3. Check next to `Project` or the subsection thereof you wish to install to
4. Select desired version
5. Click `Install`
6. Confirm
7. Wait for installation to complete
8. Ensure that `packages.config` gets committed to version control


## Override ToString

``` C#
public override string ToString()
{
    // return string representation
}
```

## Working with JSON
Install `Json.NET`, which is in package `Newtonsoft.Json`.

### Convert to JSON
Use `JsonConvert.SerializeObject(object, Formatting.Indented)` to convert an object or collection thereof to a JSON string.

### Convert from JSON
Use `JsonConvert.DeserializeObject<Type>(jsonString)` to convert from a JSON string to the desired type.
