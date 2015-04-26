# Working with User Data on Client-Side

## g_user

The `g_user` object is injected globally into client-side code to provide access to some of the properties and methods available server-side via `gs`.

### properties
* `userName`
* `userID`
* `firstName`
* `lastName`

### methods
* `getClientData({String} key)` retrieves custom session data that was attached on server side
    - Usage:
        1. server-side attaches data to the session via `gs.getSession().putClientData(key, value)`
            + this data will persist throughout the session, so consider initializing in response to the `session.established` event
        2. client-side retrieves session data via `g_user.getClientData(key)`
* `getFullName()`
* `hasRole({String} role)` returns true if user has the named role, or is admin
* `hasRoleExactly({String} role)` returns true if the user actually has the role (admin doesn't automatically get role)
* `hasRoleFromList({String} role, ...)` returns true if user has any of listed roles, or is admin
* `hasRoles()` returns true if user has any role
* `setPreference({String} key, {String} value)` sets the specified preference key value pair *on client side only*. **undocumented**
* `getPreference({String} key)` retrieves the value of the preference that was previously set with `key` via setPreference, *does not* load preference values from database. **undocumented**
* `deletePreference({String} key)` deletes the client-side vaue of the preference. **undocumented**

## globals

* `setPreference({String}name, {String} value)` can be used on client side to update a user's preference setting. This will set the preference for the current session and store the new value in the database asynchronously (? no sync warning displays). Also updates the preference map in `g_user` so that `g_user.getPreference` will retrieve this value. **undocumented**
* `getPreference({String} name)` retrieves the value of the named preference. This is performed **synchronously** but there appears to be some caching (? sync warning displays sometimes). **undocumented**

