# Client-Side Scripting

## General Guidelines

* client side scripting is javascript that runs on the browser
* use server side whenever possible
    - standardized scalable environment
    - not exposed to user
* client scripting should **support completing a form**
    - guide a user through the form, highlight next steps
    - auto fill fields based on available data
* client scripting is **not suited for security**
    - too easy to circumvent
    - Client Scripts and UI Policies only run on forms (client scripts do not run on lists, web service access, etc.)
* reduce server lookups
    - preload information whenever possible using display business rules
    - get all information with one server call (where possible)
* avoid synchronous server calls
    - complex queries will lock the browser
* avoid DOM manipulation
    - do not use gel() or getElementById()
    - use g_form methods instead

## Tips and Tricks

* g_form.setValue has an optional 3rd parameter to also set display value. This can be used to set both the actual value and the display value simultaneously, thereby eliminating the need for an AJAX call to lookup related values.

    ```javascript
    g_form.setValue(fieldName, value, displayValue);
    ```
* onChange scripts that use setValue will trigger onChange event again; make sure they don't result in an infinite recursion
* don't forget about UI Context Menus!
* use appropriate conditions for UI Actions when possible
* UI actions can be used to add new, wonderful controls to forms
* current.canWrite() can be used to check write access -- no point in running certain actions if the user has no write access
* client-side UI actions still have their conditions evaluated on the server side
* `g_user` is available client side to provide some of the options from `gs` (which is server-side)
* use UI Policies instead of client scripts where applicable