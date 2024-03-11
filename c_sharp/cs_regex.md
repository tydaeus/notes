# C-Sharp Regex
C-Sharp regex functionality operates through the System.Text.RegularExpressions.Regex. Some functionality can be achieved through the static functions of the class, while other functionality is available by instantiating a Regex object.

## Construction
`Regex regex = new Regex(string pattern [,System.Text.RegularExpressions.RegexOptions options[, timespan matchTimeout]])`

Instantiating allows specifying pattern, options, and timeout for reuse against multiple input strings, and can allow for different options using matching/replacing methods.

## Matching

### `Match()`
`Match()` functions always return a `System.Text.RegularExpressions.Match` object to store matching info about the first match. *Recommended*: use `Match` if you care only whether there's a match (not how many) or if the pattern is intended to match the whole string.

Static function:
* `Regex.Match(string input, string pattern [,System.Text.RegularExpressions.RegexOptions options[, timespan matchTimeout]])`

Instance method:
* `regex.Match(string input)`
* `regex.Match(string input, int startat)`
* `regex.Match(string input, int beginning, int length)`

### `Matches()`
`Matches()` functions return a `System.Text.RegularExpressions.MatchCollection` object storing info about all matches (empty if none found).

Static:
* `Regex.Matches(string input, string pattern[, System.Text.RegularExpressions.RegexOptions options[, timespan matchTimeout]])`

Instanced:
* `regex.Matches(string input[, int startat])`

### `Replace()`
`Replace()` functions return the string resulting from replacing the specified pattern with the specified replacement string.