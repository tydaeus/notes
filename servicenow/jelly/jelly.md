# Jelly

* **J**ava **Ele**ments originated with the Apache Foundation
* ServiceNow uses Jelly to process XML documents containing static HTML and special Jelly tags, to output web pages (Html + Javascript)

#### Use in Out-of-Box Code
Jelly is used extensively in the out of the box ServiceNow code. **Do not modify the ServiceNow jelly**. This may cause bugs, and will void auto-updating.

* **UI Pages** define the pages used throughout serviceNow
* **UI Macros** are reusable snippets that can be imported into UI Pages

## Jelly Tags

* Two kinds of tags
    - Core tags from `jelly:core` namespace ('**j** tags')
    - ServiceNow custom tags in the glide namespace ('**g** tags')

## Jelly Phases

* **Phase 1** processes j / g tags
    - substitution of `${var_name}` (JEXL - Jelly expression language) on the page occurs prior to evaluation of tags
    - Phase 1 results are **cached** for quick retrieval
* **Phase 2** processes j2 / g2 tags
    - substitution of `$[var_name]` on the page occurs prior to evaluation of tags

Variables set during phase one are not accessible during phase 2 unless explicitly transferred.

### Evaluating Jelly Expressions

`evaluate` will evaluate an expression and store its result in the specified variable.

```xml
<g:evaluate var="jvar_varname" expression="gs.getUserID();" />
User ID: ${jvar_varname}
```

For multiline expressions, can include the expression as the tag content (instead of its attribute). **The value of the *last line* will be used to set the jelly variable.**

```xml
<g:evaluate var="jvar_varname">
gs.getUserID();
</g:evaluate>
```

Declared javascript variables are also accessible throughout that jelly processing phase, including for substitution.

Setting `jelly="true"` as an attribute on the tag makes jelly variables available on the injected `jelly` object.

```xml
<g:evaluate jelly="true">
    p2.get(jelly.jvar_poll_id);
</g:evaluate>
```

### j:if
The `j:if` tag will test a JEXL expression for truth, and if the test is true, will included the specified HTML.

```xml
<j:if test="${jvar_has_voted eq true}">
        Your response was: ${response.u_answer.u_text}
</j:if>
```

### Debugging

`g:breakpoint` will cause the jelly variable values to be dumped to the debug log if debug log has been enabled. (g2:breakpoint is also available).
```xml
<g:breakpoint>
```

The gs.print and gs.log (and their scoped equivalents) are also available.
