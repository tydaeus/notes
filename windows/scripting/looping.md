# Looping in cmd scripts

## `for`

### Using `for` to iterate through files

```
:: from command-line
for /r %i in (*) do SOMETHING using %i

:: from script
for /r %%i in (*) do SOMETHING using %%i
```