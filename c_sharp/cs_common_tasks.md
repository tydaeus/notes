# C# common tasks

## Add a NuGet Package
1. Under `Tools > NuGet Package Manager` select `Manage NuGet Packages for Solution...`
2. Browse for the desired package
3. Check next to `Project` or the subsection thereof you wish to install to
4. Select desired version
5. Click `Install`
6. Confirm
7. Wait for installation to complete
8. Ensure that `packages.config` gets committed to version control

## Working with JSON
Install `Json.NET`, which is in package `Newtonsoft.Json`.

### Convert to JSON
Use `JsonConvert.SerializeObject(object, Formatting.Indented)` to convert an object or collection thereof to a JSON string.

### Convert from JSON
Use `JsonConvert.DeserializeObject<Type>(jsonString)` to convert from a JSON string to the desired type.
