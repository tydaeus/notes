# Readme - numpy

The numpy library provides much of the more complex numeric types and processing available to python.

## numpy scalar types
Unlike the Python built-in types, the numpy types allow you to specify the bit-size you want your numeric types to be. This can be necessary for interacting with various 3rd-party and system libraries.

### integer types
All numpy integer types inherit from `numpy.integer`.

#### signed integer types
These inherity from `numpy.signedinteger`.

For compatibility with C:
* 'b' - `numpy.byte` compatible with C `char` aka 
* 'h' - `numpy.short` compatible with C `short` aka 
* 'i' - `numpy.intc` compatible with C `int` aka 
* 'l' - `numpy.int_` compatible with C `long` and Python `int` aka 
* 'q' - `numpy.longlong` compatible with C `long long`
* `numpy.intp` compatible with C signed pointer

For specific bit precision:
* `numpy.int8`
* `numpy.int16`
* `numpy.int32`
* `numpy.int64`



## numpy.zeros()
Create a new array, initialized to all zeros.

`numpy.zeros(shape, dtype=float, order='C', *, like=None)`

* `shape` - `int` or `tuple(int)` defining dimensions of array
* `dtype` - data type
* `order` - whether to use C-style ('C') row-major ordering or Fortran-style ('F') column-major ordering in mem
* `like` - provided object's `__array_function__` will be used to define returned object