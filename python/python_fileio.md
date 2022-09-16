# Python File IO

## Text Files

### Read a Text File
Common usage:

```Python
with open(<filepath>, encoding='utf8') as f:
    contents = f.readlines()
```

Using `with` ensures that `f.close()` gets called when the block completes.

Encoding defaults to the result of calling `locale.getpreferredencoding(False)`, which on Windows will be `cp1252`, which may not be what is desired.
