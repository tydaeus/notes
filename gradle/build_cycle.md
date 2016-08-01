# Gradle Build Cycle

## Initialization
Gradle determines which projects are going to be part of the build, and creates a Project instance for each.

If this is a multiproject build, Gradle needs a settings.gradle file in the root project of the hierarchy. The settings file is optional 

## Configuration
Project objects are configured. Build scripts of all participating projects are executed.

## Execution
Gradle determines which tasks to execute, and executes them. This is based on the task name args passed to `gradle` command, and current dir.

