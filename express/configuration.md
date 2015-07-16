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

#####Middleware Definition

From http://expressjs.com/guide/using-middleware.html:
> An Express application is essentially a series of middleware calls.
> 
> Middleware is a function with access to the request object (req), the response > object (res), and the next middleware in line in the request-response cycle of an Express application, commonly denoted by a variable named next. Middleware can:
> 
> * Execute any code.
> * Make changes to the request and the response objects.
> * End the request-response cycle.
> * Call the next middleware in the stack.
> * If the current middleware does not end the request-response cycle, it must call next() to pass control to the next middleware, otherwise the request will be left hanging.

Middleware can be used to define how to handle specific paths, and can also be defined via the Router() factory function of the express module. The constructed router objects provide methods to handle the various types of http requests (get, post, put, etc.).

```js
var router = require("express").Router();

/**
 * @param url {String} the path to handle
 * @param handler {Function} the function to handle get requests for that path.
 * Will be provided the request and response objeccts.
 */
router.get(url, handler);
```

By convention, routes should be defined in separate files, placed in the /routes directory.

```js
/**
 * app.js
 */
var routes = require('./routes/index');
var users = require('./routes/users');

...

app.use('/', routes);
app.use('/users', users);
```

To simplify things further, the route designation can be moved from app.js to a separate file, such as routes/index.js

```js
/**
 * app.js
 */
 var routes = require("./routes");
 ...
 app.use(routes);

/**
 * routes/index.js
 */
var router = require('express').Router();

router.use(require("./main"));

module.exports = router;

/**
 * routes/main.js
 */

var router = require('express').Router();
var ctrl = require('../app_server/controllers/main');

router.get('/', ctrl.index);

module.exports = router;
```

If desired, this can be further simplified by adding logic to index.js to find and use all routes in the routes directory.

#####Page Rendering
Use the response object's render method to designate the template to be rendered to fulfill the request.

```js
function (request, response) {
    // templateName -- string containing the path and name of the view to render
    // templateValues -- object containing data to initialize the template with
    response.render(templateName, templateValues);
}
```
