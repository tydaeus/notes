# Reusing Code and Custom Libraries in Jenkins Pipeline
From https://www.jenkins.io/doc/book/pipeline/shared-libraries.

## basic `load`
The `load` step can be used to load a groovy file into your Jenkinsfile environment based on its path relative to the workspace root.

``` groovy
// ./build-util/jenkinsLib.groovy
def doSomething() {
    // do something
}

return this
// EOF

// scripted jenkinsfile
node('myNode') {
    checkout scm

    def lib
    lib = load './build-util/jenkinsLib.groovy'

    lib.doSomething()

    stage('stage 1') {
        lib.doSomething()
    }

}
// EOF

// declarative jenkinsfile
def lib

pipeline {
    stages {
        stage('stage 1') {
            script {
                lib = load './build-util/jenkinsLib.groovy'

                lib.doSomething()
            }
        }
    }
}
// EOF
```

Note that the `lib` object must be loaded before it can be used, and the source code must be checked out before it can be loaded.

Declarative pipeline doesn't currently provide a good way to declare a common "setup" step, so this can be cumbersome if using the loaded file in certain ways:
* if using in `environment` block, probalby will need to load each time and then call the method on the loaded object - I haven't found a good way to init a var prior to environment and then use it within environment statements, and environment statements don't permit multi-statements within
* using in a `stage` block requires wrapping in a `script` block. The var can be shared between stages if declared outside (as per example), but doing so (aside from using some form of lazy initializer instead of direct access) will break stage independence (i.e. restarting after the allocating stage will result in the `lib` being uninitialized and breaking that stage).



## Shared Libraries

### Defining Shared Libraries
Shared libraries are expected to be either at the root dir or a subdir of a VCS repo (git seems smoothest).

Shared library structure must match expected convention, or Jenkins compilation will not map it properly to expected output. (errors often unclear)

Expected structure is document at linked reference. Some important aspects to clarify:
* if defining an OO class named `MyClass`, it should be placed at `/src/org/foo/MyClass.groovy`
    - the class should declare `package org.foo`
    - the class name and file name must match (i.e. `MyClass`), class and filename must be camel-cased starting with a capital letter
    - there will not be a `return this` statement at tye bottom of the file
    - members declared outside the class declaration will be static members of that class, and will have access to pipeline steps
    - members declared inside the class declaration will be instance members and will not have access to pipeline steps; pipeline objects and steps can be passed into the class members, but care should be exercised if persisting state
* if defining a global var named `myVar`, it should be placed at `/vars/myVar.groovy`
    - the file does not need a `package` declaration
    - variable name and file name must match and must be camel-cased starting with a lower-case letter
    - there must be a `return this` statement at the bottom of the file
    - make the variable directly callable (i.e. make it a global function) by defining a `call` function
        - this will be called via the var name, e.g. `myVar()`
    - give the variable member functions by defining other functions, e.g. defining a `doSomething()` function allows `myVar.doSomething()`

#### Common Usecase - Define a Custom Step
To define a function that can effectively be used like a custom step, define a global variable named for the desired step, whose `call()` function performs the desired custom step.

For example, to implement `myStep`, create file `/vars/myStep.groovy`:

``` groovy
def call() { // include any params here
    // actions for step to perform
}

return this
```



### Configuring Shared Libraries
Jenkins GUI allows configuration of shared libraries at the global level or at folder level. Global libraries are "trusted" and can thus run outside the sandbox, while folder level libraries are not trusted and run inside the sandbox.

When configuring a shared library, you need to specify:
* a name for the library - this is the identifier that will be used to import the library
* the repository the library is in
* the subdir the library is in if not at root level of the repository
* the default repository version to retrieve the library from - for git this can be a commit, a tag, a branch name, etc.
    - best practice: do testing with a branch configured, so latest version of feature/spike branch will be used; specify tags in other scenarios and import a specific tagged version in Jenkinsfiles instead of using default
* how to retrieve the library via scm




### Importing Shared Libraries
Import a configured shared library with the `@Library` annotation.

If importing a shared library with class files, the annotation by convention is applied to the first `import` statement:

``` groovy
@Library('my-library')
import org.myorg.ClassName
```

If importing a shared library for its global variables (e.g. for "custom steps" as described above) the annotation is conventionally applied to the `_` symbol:

``` groovy
@Library('my-library') _
```

Import a specific version by adding the `@`: `@Library('my-library@1234') _`.

Import multiple libraries in one statement: `@Library(['my-library@1234', 'my-other-library']) _`.

Do not invoke import statements on global vars.

It's also possibly to dynamically import a library using the `library` step; this gets complex fast.


### Using Shared Libraries
Use global vars from an imported shared library by using the name of the var file. E.g. for `vars/myVar.groovy`:
* `myVar()` would invoke the `call` function
* `myVar.doSomething()` would invoke the `doSomething` function
