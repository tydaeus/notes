# Integrating Gradle with Eclipse

Gradle can be integrated with Eclipse for your programming convenience.

## Eclipse plugin
The eclipse plugin provides a good starting point to configuring gradle to work with eclipse and vice-versa.

```java
apply plugin : 'eclipse'
```

The eclipse plugin provides tasks
* `eclipse` merges gradle-specific changes into eclipse metadata
* `cleanEclipse` wipes eclipse metadata so that it can be freshly generated

Use `gradle cleanEclipse eclipse` to wipe then generate eclipse metadata so that the eclipse configuration is provided fully from gradle. This will help ensure that dependencies are resolved through gradle.
