# Python Introspection
Python provides fairly simple and robust functions for introspection.

## Attribute Checking
* `hasattr(object, attribute_name)` - returns whether the specified object has the specified attribute
* `getattr(object, attribute_name)` - returns the value object's attribute named `attribute_name`
* `setattr(object, attribute_name, value)` - sets `object`'s attribute named `attribute_name` to `value`
* `delattr(object, attribute_name)` - deletes `object`'s attribute named `attribute_name`

## Behavior Checking
* `callable(object)` - returns whether `object` can be called like a function
* `inspect.isclass(any)` - returns whether `any` is a class definition
* `isinstance(any, class)` - returns whether `any` is an instance of `class`