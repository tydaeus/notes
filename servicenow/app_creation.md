#ServiceNow Application Creation

* Define process
    - Business Problem
    - Outcomes
    - Input(s)
    - Output(s)
    - User personas/stakeholders
    - Process Steps

####Application Context

Pre-Fuji applications, and applications that are created to extend the buit-in functionality are constructed in the **global context**.

Post-Fuji applications can be built within their own applications.
* applications cannot access code/data outside their scope, as defined by their context
* applications cannot change their scope

Application scopes are named based on format:

```
x_org_my_app
```

* where the x_ indicates that it is not ServiceNow-provided application code.
* org is a 3-5 character acronym for the organization creating the application, taken from glide.appcreator.company.code system property
* the remainder is the application name, truncated to 12 chars
* the scope name is prepended to all objects created within that scope

Application code is stored separately as *Application Files*, and can be published to update sets. Applications bundle code by version, and each application version is self-contained for upgrade purposes. Applications can be *installed*, and applications can be chosen for installation from the application market.

