# Protractor - Finding Elements
Selecting elements from the page is an important part of creating Protractor tests.

## Element Locators

**Element locators** are objects that define parameters by which to retrieve DOM elements. Locators generally operate **synchronously** and can be defined and manipulated prior to the page being loaded.

Many of the functions provided by protractor return element locators (types: `ElementArrayFinder`, `ElementFinder`, `webdriver.Locator`? ...?).

### `element()`
The `element()` function defines an element locator. Element locators are not resolved until needed, so they can be defined prior to the existence of the element they will locate.

```javascript
var textField = element(by.model("fieldModel"));
```

Element locators can also be chained (`element(by.model("modelName")).element(by.model("childModelName"));`) to select children.
### `by` (`ProtractorBy`)
The global `by` object provides functions used to specify how to locate a given element. These functions are based on equivalent `WebDriverBy` functions, wrapped so that they will wait until angular is ready prior to executing. Options:
* `binding()`
* `exactBinding()`
* `model()`
* `buttonText()`
* `partialButtonText()`
* `repeater()`
* `exactRepeater()`
* `cssContainingText()`
* `options()`
* `deepCss()`

## WebElements

**WebElements** are objects that directly represent DOM elements. WebElements generally operate **asynchronously** and are only defined and manipulable after the page has finished loading.

WebElements
### `webDriver.by` (`WebDriverBy`)

