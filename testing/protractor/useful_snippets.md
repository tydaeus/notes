# Useful Snippets
Here are some snippets that may prove useful in creating protractor tests.

### Collecting Select Options
This snippet returns a promise that will resolve to an object containing the options associated with the specified select WebDriverElement, mapped to their displayed text.
```javascript
    function selectOptions(elem) {
        var done = false;
        var options = {};

        elem.findElements(by.tagName("option")).
            then(function (result) {

                for (var i in result) {
                    result[i].getText().then(function(text) {
                        options[text] = result[i];
                    });
                }

                done = true;
            });

        return browser.driver.wait(function() {
            if (done) {
                return options;
            }
        });

    }

```