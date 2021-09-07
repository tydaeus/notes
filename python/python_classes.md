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

        self
        # other statements

    # Optional string conversion method. Used to generate string representation for instances when they're referenced
    # in a context where string is expected (`print()`, `str()`, format strings).
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