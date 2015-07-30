# Getting Started with Gradle

Download gradle, unzip, place in an appropriate location, and add the bin/ dir to your path. Should be good to go. You may also want to set GRADLE_HOME to be the parent directory of your gradle.

Standard build configuration file is `build.gradle`.


## Applying Plugins

By applying plugins you can add pre-defined tasks.

```groovy
apply plugin: 'java'
/*
    Default configurations:
        sourcefiles:    src/main/java
        test files:     src/test/java
        resources:      src/main/resources
        test resrouces: src/test/resources
    Provided tasks:
        build: compiles, tests, and creates a jar file
        clean: deletes build dir
        assemble: compiles and jars, doesn't test
        check: compiles and tests
*/
```

## Repositories

Repositories allow you to retrieve dependencies, publish artifacts, or both.

```groovy
repositories {
    mavenCentral()
}
```

### Dependencies

```groovy
dependencies {
    compile group: 'commons-collections', name: 'commons-collections', version: '3.2'
    testCompile group: 'junit', name: 'junit', version: '4.+'
}
```

## Properties

List properties with `gradle properties`; this includes the properties added by plugins and their default values.

Set standard properties via declaration
```groovy
sourceCompatibility = 1.5
version = '1.0'
jar {
    manifest {
        attributes 'Implementation-Title': 'Gradle Quickstart',
                   'Implementation-Version': version
    }
}
```
