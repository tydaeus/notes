# Classes in Python

``` Python
# Declare with the class statement. Parent class can be omitted if inheriting from `object`.
class MyClass(parentClass):

    # Optional constructor receives the class definition as its first parameter. Additional parameters may be defined.
    # Constructor will usually only be used if you want to do something unusual.
    def __new__(cls, *args, **kwargs):
        # do stuff prior to returning the new instance
        # constructor must create and return the new instance somehow (or can return an arbitrary object if desired)
        # To preserve inheritance and perform standard initialization, instance creation is usually either:
        return super(MyClass, cls).__new__(cls, *args, **kwargs)
        # or:
        return object.__new__(cls, *args, **kwargs)

    # Optional initializer declares and initializes initial instance properties. This gets called automatically after
    # the instance has been created, unless the constructor is defined atypically. __init__ should not return anything.
    def __init__(self, *args, **kwargs):
        self.y = 4
        # Member identifiers prefixed with '_' are considered to be for internal use only by convention. 
        self._val1 = 3
        # Property accessors (see below) can be used to initialze internal values.
        self.val2 = 4

        # other statements

    # Optional but recommended string representation for troubleshooting purposes.
    def __repr__(self):
        return f"({repr(super)}; x: {self.x}; y: {self.y}; val1: {self.val1}; val2: {self.val2}")

    # Optional string conversion method. Used to generate string representation for instances when they're referenced
    # in a context where string is expected (`print()`, `str()`, format strings). `__repr__` will be used if `__str__`
    # is not defined.
    def __str__(self):
        return f'(x: {self.x}; y: {self.y}; val1: {self.val1}; val2: {self.val2})'

    # variables declared directly in the class statement get allocated once for the Class and once for each instance,
    # before the initializer is called.
    x = 5

    # Functions declared in the class statement are instance methods and are attached to the instance before the
    # initializer is called.
    #
    # The first parameter (if defined) will be used to represent the instance to operate on. By convention this 
    # parameter is named `self`.
    # 
    # If no first parameter defined, the function can only be called from the class definition (`MyClass`).
    #
    # Calling the class def version of the function with an object of another class as first param will effectively
    # invoke the method on that object.
    def myfunc(self):
        # do stuff to the instance via `self` variable

    

    # getter
    def get_val1(self):
        return self._val1

    # setter
    def set_val1(self, value):
        self._val1 = value

    # Create property accessor using the getter and setter (without annotation); this sets up a property object to
    # manage the logic.
    val1 = property(get_val1, set_val1)
    # with this, property can be accessed as `instance.val` for reading or writing, using the logic from get_val1 and
    # set_val1



    # getter using @property decorator
    @property
    def val2(self):
        return self._val2
    
    # setter using @property-derived decorator
    @val2.setter
    def val2(self, value):
        self._val2 = value
    
    # deleter using @property-derived decorator
    @val2.deleter
    def val2(self):
        del self._val2

    # need to know: how to provide doc string via annotation?
    # using the getter doc string appears to suffice


```

## Class dunder members

### Class definition members
* `__new__` - constructor function, called on new objects to create them, responsible for returning the resulting object. Should only be implemented for classes that need non-standard creation.
* `__mro__` - method resolution order. Class property that indicates the class hierarchy order used for method call resolution.
* `__instancecheck__` - function used to check whether an instance is a member of the class. Override for non-standard instance checking. This is used when `isinstance()` is invoked.
* `__subclasscheck__` - function used to check whether a class is a subclass of this class. Override for non-standard subclass checking. This is used when `issubclass()` is invoked.

### Instance members
* `__init__` - initializer method, called on all new objects after construction and before they are provided to caller. Should generally be implemented.
* `__repr__` - string representation method. Should return a meaningful string representation of the instance, i.e. for use during debugging. Is called when `repr(instance)` is invoked.
* `__str__`  - 'pretty' string representation method. Should be reasonably formatted and omit any esoteric fields, for user-friendly representation. Is called when `str(instance)` is invoked.
* `__class__` - represents the instance's class. In Python3 this will be the same value as returned by `type(instance)`.


## Interfaces
(source: https://realpython.com/python-interface/)

Python doesn't support interfaces in the same way that C-family languages do, but it can still be useful to provide interface-analogs when developing larger projects.

### Informal Interfaces
The simplest option is to provide an informal interface for use as a superclass.

``` Python
class InformalInterface:
    def do_something(self, arg1, *args):
        "Do something useful"
        pass

class Implementation(InformalInterface):
    def do_something(self, arg1, *args):
        "Overrides InformalInterface.do_something"
        # actually do something
```

This allows for defining some amount of structure, and allows for using `issubclass(Implementation, InformalInterface)` to check if a given implementation is a `InformalInterface`. However, this doesn't do anything to enforce that implementations actually implement the methods defined in the interface, it just checks that they're part of the hierarchy. It also doesn't play nicely with duck-typing.

