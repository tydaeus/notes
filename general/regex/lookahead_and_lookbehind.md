# Lookaheads and Lookbehinds
Lookaheads and lookbehinds allow for more regex mis/matching options, but do apply a greater processing cost.

## Lookaheads
Lookaheads allow for mis/matching on text based on what follows it, without capturing this following text. 

### Negative Lookahead
Negative lookaheads look for a phrase that is not present.

Create a negative lookahead matcher with `(?!)`, inserting the phrase to negate after the exclamation point. E.g.:

```js
/foo(?!bar)/
// find "foo" not followed by "bar"

"foobar fooboo".replace(/foo(?!bar)/, "fiz");
// returns "foobar fizboo"
```

### Positive Lookahead
Positive lookaheads allow for matching text that follows the desired phrase, without making this text part of the resulting match.

Create a positive lookahead matcher with `(?=)`, inserting the phrase to
match after the equals sign. E.g.:

```js
"foobar fooboo".replace(/foo(?=bar)/, "fiz");
// returns fizbar fooboo
```

## Lookbehinds
Lookbehinds allow for mis/matching on text based on what precedes it, without capturing this preceding text.

### Support Notes
* Perl, Python, and Boost only allow fixed-length strings, and all alternative patterns must be of same length
* PCRE, PHP, Delphi, R, and Ruby only allow fixed-length strings, but alternative patterns may vary in length
* Java allows finite repetition. Known bugs exist in lookbehinds prior to Java 6.
* JavaScript, std::regex, and Tcl do not support lookbehind

### Negative Lookbehind
Negative lookbehinds look for a phrase that is not present preceding the target text.

Create a negative lookbehind matcher with `(?<!)`, inserting the phrase to negate after the exclamation point.

### Positive Lookbehind
Positive lookbehinds look for a phrase that is present preceding the target text.

Create a positive lookbehind matcher with `(?<=)`, inserting the phrase to match after the exclamation point.
