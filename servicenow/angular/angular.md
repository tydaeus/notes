# Developing Modern Applications with AngularJS

### Adding Angular to ServiceNow

1. Open UI Scripts
2. Create New UI Script, named something along the lines of angular_1_3
3. Put a script reference to the angular CDN into the script content, or paste in the desired minified angular code, as a document.writeln

```javascript
document.writeln('<script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.3.14/angular.min.js"></script>');
```

Additional scripts that will be used in multiple places should also be defined as UI scripts, e.g. controllers, services, directives, etc.

### Creating Pages Using Angular

To add Angular to a page, create a new UI page with a reference to the UI script.

```javascript
<script language="javascript" src="x_scope.angular_1_3.jsdbx" />
```

Additional shared scripts that use angular can then be included in a similar manner.

Complete the page using standard html with angular markup.

### Connection Angular app to the ServiceNow Backend


