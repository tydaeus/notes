# Getting Started with Gradle

Download gradle, unzip, place in an appropriate location, and add the bin/ dir to your path. Should be good to go. You may also want to set GRADLE_HOME to be the parent directory of your gradle.

Standard build configuration file is `build.gradle`.

## Commandline Commands

### Properties
`gradle properties` lists all set and set-able properties for the gradle.build file in CWD.

### Tasks
`gradle tasks` lists all available tasks for the gradle.build file in CWD.

## Applying Plugins

By applying plugins you can add pre-defined tasks.

```java
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

Plugin functionality is inheritable and idempotent, i.e. using the 'war' plugin means the 'java' plugin is not necessary, but applying 'java' on top of 'war' does not result in doubling application of the 'java' plugin.

## Repositories

Repositories allow you to retrieve dependencies, publish artifacts, or both.

```java
repositories {
    mavenCentral()
}
```

### Dependencies

Gradle downloads dependencies when they're needed. For example, all 'compile' dependencies will be downloaded before compilation occurs.

```java
dependencies {
    compile group: 'commons-collections', name: 'commons-collections', version: '3.2'
    testCompile group: 'junit', name: 'junit', version: '4.+'
}
```

Dependencies are downloaded to `"$USER_HOME"/.gradle` subdirectory by default. This can be reconfigured with the `GRADLE_USER_HOME` environment variable, `gradle.user.home` system property, or `--gradle-user-home` command line parameter.

## Properties

List properties with `gradle properties`; this includes the properties added by plugins and their default values.

Set standard properties via declaration
```java
sourceCompatibility = 1.5
version = '1.0'
jar {
    manifest {
        attributes 'Implementation-Title': 'Gradle Quickstart',
                   'Implementation-Version': version
    }
}

sourceSets {
    main {
        java {
            // define source code directory
            srcDirs = ['src']
        }
    }
    test {
        java {
            // define test code source dir(s)
            srcDirs = ['test']
        }
    }
}

buildDir = 'dist'
```

## Configurations
Configurations are used to group settings. For example, compile is used for compilation dependencies, and testCompile is used for test compilation dependencies.

## Tasks

## Gradle Wrapper
The gradle wrapper allows auto-bundling of the gradle runtime with a project so that the project can be ported to other environments and gradle will automatically be downloaded and configured as necessary.

```java
task wrapper(type: Wrapper) {
    gradleVersion = '2.5'
}
```

Running this task will generate scripts 'gradlew' and 'gradlew.bat' which should be checked into version control, and can then be used in place of the gradle command to perform gradle tasks (they will download the named version of gradle to achieve this).

The wrapper can be configured to use a specific (e.g. enterprise) repository via the `distributionUrl` property, and to place its downloaded gradle version in a specific location via the `distributionPath` property (relative to GRADLE_HOME).

## The Gradle Daemon
The gradle daemon lurks in the background, waiting to improve build speed. Good for development environments, bad for build servers (Jenkins).

Enable/disable temporarily with `--daemon`/`--no-daemon`. Enable for environment by either adding `-Dorg.gradle.daemon=true` to `GRADLE_OPTS`, or adding `org.gradle.daemon=true` to the `"$GRADLE_USER_HOME"/gradle.properties

## Continuous Build
Use `-t` or `--continuous` along with one or more task invocations to cause gradle to remain running and auto-repeat the tasks when changes are detected. E.g. `gradle -t test` . This feature is currently "in incubation" and may cause issues, especially on Macs.

## Running the built program

```
$java -cp build/classes/main package.Main
```