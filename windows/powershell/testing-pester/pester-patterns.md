# Pester Patterns
Some patterns I've learned through experimentation with and reading about Pester, to make test writing and maintenance easier.

## Defining Shared Parameters
In most tests you should define common input parameters and expected output values that can be used throughout the testing.

- **Test environment parameters** - should get defined separately from input parameters and expected output, so they can be overridden separately and derived values casn be derived from them. Consider using a script to define them.
- **Default input parameters** are easiest to work with in a HashTable (to allow splatting). *However*, as complex objects, HashTable contents don't get the scoping read-only treatment when defined in `BeforeAll` blocks that primitive values do. When defining default input parameters, either:
    + Provide a function/ScriptBlock to generate the input parameters within a `BeforeAll` block, or
    + Define default input parameters only within a `BeforeEach` block, so that they get reset between tests
    + Consider keeping the input values describing the test environment used to determine the script input parameters separate and deriving the script parameters from them
- **Expected output parameters** can often be defined as primitive values within a `BeforeAll` block. *However*, in many cases expected output will depend directly on provided input. Consider providing an **expected output function/ScriptBlock** that operates on the input parameters to describe expected output in a way that documents the input to output relationship (e.g. if `-Name` gets combined with `-OutputPath` to generate the output file location)
- Make sure that shared Mocks get defined in a `BeforeEach` block instead of a `BeforeAll` block, unless none of the tests using them redefine mocks. 

By following these practices, nested testing levels can use or override the testing variables without colliding with tests outside their structure.

## Test in Foundational Order
Write the tests and set them up to run in order from simplest and most foundational to most complex and esoteric. This means tests on basic input parameter, output validation, and error checking should get written and should be run before tests that handle more complex logic (edge cases, business logic, etc.). This results in the first failing test being the most indicative of what's going wrong -- if basic i/o or error checking isn't occurring correctly, that will likely interfere with the higher complexity functionality.