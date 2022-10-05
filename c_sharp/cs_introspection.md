# Introspection in C#

## Information About Callers
Retrieving information about where a function was called from is very easy in recent versions of C# (.Net Framework 4.5+?); attributes can be applied to optional arguments to allow the compiler to insert caller info. From https://docs.microsoft.com/en-us/dotnet/api/system.runtime.compilerservices.callermembernameattribute?redirectedfrom=MSDN&view=net-5.0:

``` C#
public void TraceMessage(string message,
        [System.Runtime.CompilerServices.CallerMemberName] string methodName = "",
        [System.Runtime.CompilerServices.CallerFilePath] string sourceFilePath = "",
        [System.Runtime.CompilerServices.CallerLineNumber] int sourceLineNumber = 0)
{
    System.Diagnostics.Trace.WriteLine("message: " + message);
    System.Diagnostics.Trace.WriteLine("member name: " + methodName);
    System.Diagnostics.Trace.WriteLine("source file path: " + sourceFilePath);
    System.Diagnostics.Trace.WriteLine("source line number: " + sourceLineNumber);
}
```

## `GetType()`
Use `GetType()` to get a Type object representing a given object's type:

``` C#
Type type = obj.GetType();
```

### `type.GetProperties()`
`type.GetProperties()` returns a collection of `PropertyInfo` objects representing the object's properties. `propertyInfo.GetName()` returns the name of a property, and `propertyInfo.GetValue(object)` returns its value, e.g.:

``` C#
Type type = obj.GetType();

StringBuilder result = new StringBuilder();
result.Append("[")
    .Append(type.FullName)
    .Append("]: {");

bool firstProp = true;

foreach (PropertyInfo prop in type.GetProperties())
{
    // provide comma and space before all properties except first
    if (firstProp)
    {
        firstProp = false;
    } else
    {
        result.Append(", ");
    }

    result.Append(prop.Name)
        .Append(":\"");

    object value = prop.GetValue(obj);

    if (value == null)
    {
        result.Append("null");
    }
    else
    {
        result.Append(value);
    }

    result.Append("\"");
}

result.Append("}");
return result.ToString();
```


## Information About Assemblies
Load the assembly with `System.Reflection.Assembly.LoadFrom(<Name>)` or `.LoadFile(<FilePath>)`.

Methods and properties on the resulting `RuntimeAssembly` object can be used to retrieve information about it, including:

* `ExportedTypes` - list of visible types defined by the assembly
* `GlobalAssemblyCache` - whether the assembly is registered in the GAC
* `GetReferencedAssemblies()` - assemblies referenced by the assembly