#Express Configuration
Most configuration of an express application starts in the app.js file. However, for larger projects, it may be necessary to move some configuration elsewhere.

###Changing Core Directories
```js
app.set('views', path.join(__dirname, 'views'));
// can change to:
app.set('views', path.join(__dirname, 'app_server/views'));
```

###Routes and Controllers
Under default setup, controllers are part of the routes. If you want a more MVC style setup, it may be advisable to separate these concerns, so controllers handle application logic while routes map urls to controllers.

#####Route Definition
Routes are defined via the Router() factory function of the express module. The constructed router objects provide methods to handle the various types of http requests (get, post, put, etc.).

```js
var router = require("express").Router();

/**
 * @param url {String} the path to handle
 * @param handler {Function} the function to handle get requests for that path.
 * Will be provided the request and response objeccts.
 */
router.get(url, handler);
```

#####Page Rendering
Use the response object's render method to designate the page to be rendered to fulfill the request.

```js
function (request, response) {
    // templateName -- string containing the path and name of the view to render
    // templateValues -- object containing data to initialize the template with
    response.render(templateName, templateValues);
}
```
