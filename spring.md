#Spring Notes

####Lifecycle Annotations

######PostConstruct
The PostConstruct annotation specifies that the annotated method should be called once the spring context has been initialized, after the bean is ready. (can be applied to non-bean types as well);

```java
@PostConstruct
public void init() {
    // perform initialization
}
```