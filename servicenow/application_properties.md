# Application Properties

*Application Properties* allow the creation of variables that can be referenced elsewhere (Similar to environment/system properties). **Do not use Insert & Stay with application properties** - there is a bug in Fuji that causes the newly created record to scope improperly.

1. Define application properties by attaching them to the appropriate category in **System Properties > Categories**
2. Make application properties configurable by creating a new application module with appropriate access controls (generally admin-only)
    * menu link should be type **URL (from arguments)**
    * arguments should be of format: `system_properties_ui.do?sysparm_title=<Category Title>&sysparm_category=<Category Name>`
3. use `gs.getProperty("<PropertyName>")` to retrieve the property value