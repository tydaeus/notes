# Jasmine and Angular


## Including Modules

Modules must be included prior to any expectations and before any Angular injections occur. Include modules with the global `module` function. The module containing the unit under test must be included, and so must any modules whose components may be shadowed by provided dependencies. Angular will auto-resolve other dependencies.

```javascript
beforeEach(module("module1"));
```

## Providing Dependencies

The unit's dependencies can be shadowed with mocked, spied, or dummied dependencies using the $provide function.

Provide must be called after other module calls and prior to any injector use.

```javascript

// setup the dummy, mock, or spy

beforeEach(module(function($provide) {
    $provide.factory("serviceName", function() { return dummyService; });
}));
```

## Inject As-Is Dependencies

Retrieve any angular services or mock services with the `inject` function. This allows referencing/manipulating these services within tests.

```javascript
var service;
beforeEach(inject(function(_service_) {
    service = _service_;
}));

// or

beforeEach(inject(function($injector) {
    service = $injector.get("service");
}));
```

## Inject the Unit

Inject the unit under test using the `inject` function.

```javascript
var unit;

beforeEach(inject(function(_unit_) {
    unit = _unit_;
}));
```

## Template

```javascript

describe("serviceOrControllerName serviceOrControllerType", function() {
    var unit;

    // necessary angular service variable placeholders
    var $q, $httpBackend, ...

    // necessary custom services
    var myService, ...

    // get necessary modules
    beforeEach(function() {
        module("module1");
        ...
    });

    // provide dummied/mocked/spied dependencies
    beforeEach(module(function($provide) {
        var dummyService = ...

        $provide.factory("service", function() { return dummyService; });
        ...
    }));

    // inject needed services; may want to separate Angular from custom
    beforeEach(inject(function(_$q_, _$httpBackend_, ...) {
        $q = _$q_;
        $httpBackend = _$httpBackend_;
    }));

    // inject unit being tested
    beforeEach(inject(function(_unit_) {
        unit = _unit_;
    }));
});