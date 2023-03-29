# PowerShell Regex

## The `-match` Operator
Use the `-match` operator to check whether a string(s) matches a regex. E.g. `$str -match $regex` will return a boolean indicating whether the string contained in `$str` matches the regex contained in (string variable) `$regex`.

If the first operand is a collection of strings, a collection of the matching strings will be returned.

The `$Matches` automatic Hashtable variable will be populated with the first match for each regex grouping if `-match` returns true, if matching a single string. `$Matches` is not updated if matching a collection.

The `-notmatch` operator performs the negation equivalent to `-not ($str -match $regex)`, and populates `$Matches` if matches were present.

### Named Capture Groups
You can specify a name for each capture group by starting the capture with `?<GroupName>` (angle brackets are part of the naming), which then will allow you to retrieve the matched group from the `$Matches` hashtable. E.g.:

``` PowerShell
$addressLine = 'St. Somewhere, PA 12345'

if ($addressLine -match '^(?<City>[^,]+), (?<State>[A-Z]{2}) (?<ZIP>\d{5}(-\d{4})?)') {
    $city = $Matches['City']
    $state = $Matches['State']
    $zip = $Matches['ZIP']
}

```



## The `-replace` Operator
(notes based on https://vexx32.github.io/2019/03/20/PowerShell-Replace-Operator/)

Use the `-replace` operator to perform regex substitution on a string; syntax is `<STRING> -replace <REGEX>,<REPLACEMENT>`. All places where `<STRING>` matches `<REGEX>` will be replaced with `<REPLACEMENT>`.

`<REPLACEMENT>` can use standard regex numbered replacement groups, e.g. `'foo(1234,56789)' -replace '^.*\((\d+),.*$','$1'` results in `'1234'` (the whole string gets matched, but the first parenthetical group is the only piece kept in the replacement string). The `$0` replacement group represents the entire matched string. Be aware that the `$` must be escaped so that PowerShell doesn't attemt to interpret it as a variable reference.

A name can be specified for a replacement group in format `?<Name>` at the beginning of the replacement sequence, e.g. `'foo(1234,56789)' -replace '^.*\((?<Number>\d+),.*$','${Number}'`. Note that the name must be wrapped in `{}` in the replacement string.


## Regex Type
The `[regex]` object type can be used as an alternative to the `-match` operator. Use `[regex]::Matches($str, $regex)` to retrieve a collection of matches.

### `Regex.Escape` method
Use the `Escape` static method to escape all regex characters in a string. `[Regex]::Escape($string)`.