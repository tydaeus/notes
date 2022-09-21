# PowerShell Operators

## `-f` - String Formatting
`<string with placeholders> -f <value array corresponding to placeholders>`

Placeholders follow format `{<index>[,alignment][:format]}`, where:
* index - the index of the position in the value array to use
* alignment - how many characters to align to (positive for right align, negative for left align)
* format - a format string indicating how to format the data

Common Format Strings
* `c` - currency format
* `d[N]` - numeric paddeded to 'N' length, with leading 0s added if needed
* `e` - scientific notation
* `f[N]` - fixed point with 'N' places
* `g[N]` - compact format either fixed or scientific, with 'N' places
* `n[N]` - numeric with thousands separator, 'N' decimal places
* `p[N]` - percentage (e.g. .5 becomes "50%), 'N' decimal places
* `x` - hex format
* `#` per digit - free-form numeric digit placement, e.g. `{0:###-####} -f 1234567` yields `123-4567`. Numeric values only, doesn't populate missing digits, appears to fill from right to left without any padding (above string on `12` yields `-12`).

If the value is a date type, it will be formatted using (case-sensitive) date format string rules instead of the standard format strings.

**Common gotcha**: Be sure to wrap the formatting operation in parentheses if there's anything else on the right-hand side of the statement; otherwise, the `-f` may be taken for a parameter, or other exciting misinterprations may occur without error.

Source: https://ss64.com/ps/syntax-f-operator.html