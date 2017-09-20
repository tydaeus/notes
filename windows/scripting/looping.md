# Looping in cmd scripts


## `for`
Notes based off of <https://ss64.com/nt/for.html>. For full details, reference this url.

The general form is `for %%PARAM in (SET) do COMMAND`. Different `/` options allow specifiying what kind of set is being used.

The `for` command behaves (somewhat) like a for loop.

Note that in scripts, `for` parameters are referenced with `%%`, while on the command line they are referenced with `%`. My notes will assume `for` is being used in a script.

### `for` parameters
The specified `for` parameter must be a single char, e.g. `%%G`.

If the `in` clause provides a single result, `%%G` will be used alone. If the `in` clause provides multiple results, additional letters will be used, in alphabetical order (e.g. `%%G`, `%%H`, `%%I`...).

If a `for` param refs a file, parameter extension can be used similar to filenames provided as invocation params.

You can pick any letter of the alphabet, but `%%G` is a good choice because:

* it does not conflict with any of the parameter extension options
* it provides a long run of consecutive letters that do not conflict with extension options
* extension options are case sensitive, so the capital letter is good (so `%%A` is fine, as long as you are careful to use upper case)

### Using `for` to iterate through files

```cmd
:: iterate through the files in cwd
::      /r iterates through files rooted at the specified path
::      without a path specified, it uses the current directory
for /r %%i in (*) do (
    echo %%i
)
```

### Using `for` to iterate through parameters

