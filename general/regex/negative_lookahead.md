## Negative Lookahead
Negative lookaheads look for a phrase that is not present.

Create a negative lookahead matcher with `(?!)`, inserting the phrase to negate after the exclamation point. E.g.:

```js
/foo(?!bar)/

// find "foo" not followed by "bar"
```

## Positive Lookahead
Positive lookaheads allow for matching text that follows the desired phrase, without making this text part of the resulting match.

Create a positive lookahead matcher with `(?=)`, inserting the phrase to
negate after the equals sign. E.g.:

```js
"foobar".replace(/foo(?=bar)/, "fiz");

// returns fizbar
```
