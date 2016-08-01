# Using JUnit

## Assertions
Assertions compare values to expectations:

- assertEquals(expectedValue, actualValueVar)
- assertNull
- assertNotNull
- assertSame(expectedCollection, actualCollectionVar)

Assertions are static methods attached to the org.junit.Assert class. Make life easier by importing them statically:
```java
import static org.junit.Assert.*;
```

## Testing Exceptions

### Try/Catch - fail
```java
...
try {
    unit.doThing();
    Assert.fail("expected exception");
} catch(Exception e) {
    // success
}
...
```

### Expected
```java
@Test(expected=Exception.class)
public void test_the_thing() {
    unit.doThing();
}
```
