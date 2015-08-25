# Annotations

Annotations allow for providing additional data about a declared class or member. This can be used for documentation purposes, compiler warnings, or allowing annotated classes/members to be designated for special runtime processing.

## Defining a Custom Annotation

```java
// example provided by Oracle's java tutorials
@interface ClassPreamble {
   String author();
   String date();
   int currentRevision() default 1;
   String lastModified() default "N/A";
   String lastModifiedBy() default "N/A";
   // Note use of array
   String[] reviewers();
}
```

## Meta-Annotations

Meta-annotations are annotations that apply to other annotations. (Source: the Java tutorials)

* `@Retention` specifies how the marked annotation should be stored
    - `RetentionPolicy.SOURCE` – The marked annotation is retained only in the source level and is ignored by the compiler.
    - `RetentionPolicy.CLASS` – The marked annotation is retained by the compiler at compile time, but is ignored by the Java Virtual Machine (JVM).
    - `RetentionPolicy.RUNTIME` – The marked annotation is retained by the JVM so it can be used by the runtime environment.
* `@Documented` specifies annotation should be included in javadoc
* `@Target` annotation marks another annotation to restrict what kind of Java elements the annotation can be applied to. A target annotation specifies one of the following element types as its value:
    - `ElementType.ANNOTATION_TYPE` can be applied to an annotation type.
    - `ElementType.CONSTRUCTOR` can be applied to a constructor.
    - `ElementType.FIELD` can be applied to a field or property.
    - `ElementType.LOCAL_VARIABLE` can be applied to a local variable.
    - `ElementType.METHOD` can be applied to a method-level annotation.
    - `ElementType.PACKAGE` can be applied to a package declaration.
    - `ElementType.PARAMETER` can be applied to the parameters of a method.
    - `ElementType.TYPE` can be applied to any element of a class.
* `@Inherited` annotations can be inherited by subclasses (applies at class level only)
* `@Repeatable` *(Java 8 Only)* can be applied multiple times to the same annotatee