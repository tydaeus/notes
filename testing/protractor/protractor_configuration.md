# Configuring Protractor

```javascript
(function() {
    "use strict";

    /* global exports, require, browser, by */

    var baseUrl = url_to_access_application_at;

    exports.config = {
        // where to view the server
        seleniumAddress: url_selenium_webdriver_manager_was_set_to_serve_at,
        // what test files to run
        specs: array_of_filenames_or_patterns_to_match,

        baseUrl: baseUrl,

        // what element ng-app is declared on (defaults to body)
        // if not set appropriately, will generate:
            // Error: Error while waiting for Protractor to sync with the page: {}
        rootElement: "#appDeclaration",

        // how long to wait (ms) for Protractor to synchronize -- waiting until all timeouts and http requests finished
        // does not wait on intervals, so $interval should be used in place of $timeout for repetitive timeouts
        // default 11000
        allScriptsTimeout: 30000,

        // function that gets called prior to beginning testing (once)
        onPrepare: function () {

            browser.driver.get(url_for_login_page);

            browser.driver.findElement(userid_elment).sendKeys(userid_string);
            browser.driver.findElement(password_element).sendKeys(password_string);
            browser.driver.findElement(login_button_element).click();

            // wait until page is redirected so that we know login is complete
            browser.driver.wait(function() {
                return browser.driver.getCurrentUrl().then(function(url) {
                    return /home/.test(url);
                });
            });
        }
    };

}());
```