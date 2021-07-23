# C# Standard Exceptions
Excerpted and condensed from https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/using-standard-exception-types.

Other than Exception, only the following built-in Exceptions should be thrown or extended by developer code (the remainder are for use by the runtime):

* `InvalidOperationException` if the object is in an inappropriate state.
* `ArgumentException` or one of its subtypes if bad arguments are passed to a member. Prefer the most derived exception type, if applicable
    - Set the `ParamName` property when throwing one of the subclasses of `ArgumentException`. This property represents the name of the parameter that caused the exception to be thrown. Note that the property can be set using one of the constructor overloads.
    - Use `value` for the name of the implicit value parameter of property setters.