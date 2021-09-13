# Python Tricks
Here are some tricks to doing things in Python that may be desirable, or that you may be accustomed to doing in other languages.

## Anonymous Objects
(from https://stackoverflow.com/a/42816745/2939139)

Use a lambda and the `type` function to generate an object from a keyword set:

``` Python
Anon = lambda **kwargs: type("Object", (), kwargs)()

# Usage example
coords = Anon(x = 5, y = 8)
coords.x
# 5
```

## Comparing Contents of Different Iterables
Compare by converting both to `tuple`.

```Python
same = tuple(iter1) == tuple(iter2)
```
