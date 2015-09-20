#Introspection

Introspection allows for inspecting a java class that was created in accordance with the java bean api and retrieving bean api relevant data about it.

##Introspector

`Introspector` is the java class that introspection powers are attached to. Use `Introspector.getBeanInfo()` methods to retrieve information about a given class.

###Introspection process

The introspection process first looks for a class with `BeanInfo` appended to it to see if formally declared data is available. E.g., introspecting `Clazz` will look for `ClazzBeanInfo`. The searchPath can be viewed with `Introspector.getBeanInfoSearchPath()` and set with `Introspector.setBeanInfoSearchPath(String[] searchPath)`; this path takes the form of a list of packages to search.

If beanInfo is not available within the class's package, or within a package within the beanInfo search path, then beanInfo is generated through a simple reflection.

###The getBeanInfo Methods

* `getBeanInfo(Class <?> beanClass)` retrieves info for the bean and all superclasses
* `getBeanInfo(Class <?> beanClass, int flags)` retrieves info for bean and all superclasses, subject to control flags
* `getBeanInfo(Class <?> beanClass, Class<?> stopClass)` retrieves info for the bean and all superclasses below `stopClass`
* `getBeanInfo(Class <?> beanClass, Class<?> stopClass, int flags)` retrieves info for the bean and all superclasses below `stopClass`, subject to control flags

Introspector caches retrieved information about classes for use in subsequent queries.

####Control flags
The control flags are defined as constants on Introspector
* IGNORE_ALL_BEANINFO
* IGNORE_IMMEDIATE_BEANINFO - ignores beanInfo specific to the passed bean class
* USE_ALL_BEANINFO

##Using BeanInfo

###PropertyDescriptors
The bean's properties are listed in the bean info as a `PropertyDescriptor[]`, retrievable via `beanInfo.getPropertyDescriptor()`.
* 

