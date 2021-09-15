# Python Introspection
Python provides fairly simple and robust functions for introspection.

## Attribute Checking
* `hasattr(object, attribute_name)` - returns whether the specified object has the specified attribute
* `getattr(object, attribute_name)` - returns the value object's attribute named `attribute_name`
* `setattr(object, attribute_name, value)` - sets `object`'s attribute named `attribute_name` to `value`
* `delattr(object, attribute_name)` - deletes `object`'s attribute named `attribute_name`
* `vars(object)` - list variables defined on `object`
* `dir(object)` - list functions defined on `object`

## Behavior Checking
* `callable(object)` - returns whether `object` can be called like a function
* `isinstance(any, class)` - returns whether `any` is an instance of `class`
* `inspect.isclass(any)` - returns whether `any` is a class definition
* `inspect.getsource(function)` - returns the definition for `function`, if source is available
    + `dill.source.getsource(function)` -  requires 3rd-party `dill` library, can potentially go a little deeper than `inspect.getsource`