# Reflection

Reflection allows java to retrieve information about classes, objects, and members during runtime. Reflection incurs some overhead, and should be used only when necessary.

## Getting Class objects

Retrieving a Class object is the entry point to reflection. There are several ways to do this, depending on what information is currently available.

* `referenceType.getClass()` returns a reference types class. In the case of implemented interfaces, it returns the class of the implementing object.
* `primitiveType.class` references a primitive's class, e.g. `boolean.class`
* `package.Class.class` retrieves a class's class
* `Class.getName(String className)` retrieves the class object for a fully qualified class name, or for an array type using syntax described in the `Class.getName` documenation
* `WrapperClass.TYPE` retrieves the class of the wrapped primitive for a wrapper type, e.g. `Double.TYPE`

## Getting More info About Classes

* `class.getSuperclass()`
* `class.getClasses()` retrieves all *public* member classes, interfaces, and enums declared within the class, including inherited members
* `class.getDeclaredClasses()` retrieves all member classes, interfaces, and enums *declared* within the class, including private members

## Retrieving parent class from members
* `class.getDeclaringClass()`
* `field.getDeclaringClass()`
* `method.getDeclaringClass()`
* `constructor.getDeclaringClass()`
* `class.getEnclosingClass()` retrieves the immediately enclosing class; anonymous classes do not have declaring classes, but do have enclosing class
