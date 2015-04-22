# Server-Side Scripting

## Javascript Context and Naming Collisions

#### Global vs. Local vs. IIFE (Immediately Invoked Function Expressions);

```javascript
	var c = 'my comment';
	current.comments = c;

	///////////////////////

	setComment();

	function setComment() {
	    var c = 'my comment';
	    current.comments = c;
	}

	///////////////////////
	(function() {
	    var c = 'my comment';
	    current.comments = c;
	})();

```

###### Fuji Business Rule Wrappers
All business rules in Fuji and newer use function wrappers `onBefore()`, etc. This is enforced through a special flag attached to all business rules that is true for Fuji+ rules, and false for older rule. (`_execute`?)

## Debugging Business Rules and Script Includes

Server-side javascript debugger was added in Dublin.

Enable the javascript debugger by opening the gear menu on the right side of the banner and selecting "Javascript Log and Field Watcher".

#### Referenced vs. De-referenced Values

When retrieving fields from an object, as in `arr.push(obj.property)`, if property is an object, only a reference will be stored. So, if obj.property changes, the values stored in the array will change with it.

So, the value must be de-referenced. Options:
```javascript
arr.push(obj.property.toString());
/* converts property to string, but throws an error if property doesn't have a toString() method */

arr.push(obj.property + "");
/* converts to string by concatenating an empty string onto it; null-safe */

arr.push(g_form.getValue) // check syntax on this, I don't know it
```

## Unified Code through Script Includes

#### Script Includes Enable Re-usable Code

Functions or classes can be defined in Script Includes for use throughout server-side code.

The PrototypeJS library is included throughout ServiceNow to simplify class/object management.

## Use GlideAggregate for Faster Stats

#### GlideAggregate
* uses SQL "group by" and "having" clauses to calculate aggregate data at database (not JVM)
* improves calculation time compared to GlideRecord
* Reduces data sent to JVM
* much more efficient than getRowCount(); scales better as row count increases

#### getRowCount is Evil

##### check if query has records
Instead of
```javascript
if (gr.getRowCount() > 1) {
    // do something
}
```
use
```javascript
if (gr.hasNext()) {
    // do something
}
```
Both will check if any records were found, but getRowCount iterates over the full set.

##### find duplicates in the CMDB (example)
```javascript
var ciGA = new GlideAggregate("cmdb_ci");
ciGA.addNotNullQuery("name");
ciGA.groupBy("name");
ciGA.groupBy("sys_class_name");
ciGA.addAggregate("COUNT");
ciGA.addHaving("COUNT", ">", 1);
ciGA.query();
while(ciGA.next()) {
    gs.print(ciGA.getAggregate("COUNT") + "x " + ciGA.name + 
        " [" + ciGA.sys_class_name + "]");
}
```
