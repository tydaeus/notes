# Technical Best Practices

(Also available through the wiki)

## Business Rules Best Practices

* **Display**
    - runs just before form is loaded
    - typical use: pass g_scratchpad info to client scripts
* **Before**
    - runs just before DB action
    - implicit update - **don't use `current.update()`**
    - generally used for manipulating *current record*
* **After**
    - generally used for managing *related* tables/fields
    - generally *don't* use `current.update`
* **Async**
    - consider as replacement for after rules
        + Is the updated info required on screen immediately?
    - examples:
        + Processing Email
        + Calculating SLAs
        + Generating report fields
    - creates a scheduled job
    - no previous object available

#### Avoid Global Business Rules
*   loaded for *every* page
*   use script includes instead
    -   only loaded on as needed
    -   easy to convert
*   not allowed in Fuji

#### Encapsulate Code in Functions
**Especially for business rules!**
* all variables and functions are by default global
* multiple business rules running with same var names can have unpredictable conflicts
* Fuji automatically encloses business rule code in functions
    - these functions get properly scoped and called
    - e.g. onBefore()
* Self-executing anonymous functions are also good

#### Other Business Rules Best Practices
* Make business rules small and specific
    - easier to debug and maintain
* Use conditions to prevent unnecessary execution and simplify debugging
    - debugging output will indicate which rules are corrected and which are skipped
    - notable exception: conditional event handlers

## Client Script Best Practices
* runs on browser
* when well designed it can reduce time to complete a form
* use server side when possible
* client scripts and UI policies run on **forms only**
    - may want to disable list editing (via ACL) if must use client scripting
* minimize server lookups

####Getting Server Data
######Options to get server data to client (ordered from most preferred to least)
1. g_scratchpad
2. GlideAjax
3. g_form.getReference()
4. GlideRecord Query

#####g_scratchpad
* great for passing info to form (on load)
* no round trip required
* does not update fields dynamically (onChange)
* can allow access to fields that are not included in the form

#####GlideAjax
* **Pros**
    - async callback functionality
    - more flexibility in what's returned
    - can be used for dynamic updates
* **Cons**
    - a little more complex to create
        + client *and* server side scripts to write/maintain
    - requires a round trip from client to server 

######using setValue() with Reference Fields
* Usage: g_form.setValue(fieldName, value, displayValue);
    - **if only two params, will cause a synchronous AJAX call to look up the displayValue**
    - use GlideAjax or getReference if needed

#####Avoid DOM manipulation
* avoid getElementById() or gel() calls
* use g_form methods to prevent browser compatibility issues
    - g_form.getControl()
    - g_form.setValue()

## Coding Best Practices

##### Comment Your Work
* get into the habit
* you **will** forget why you did something

##### Avoid Hard-Coded Values
* Avoid using hard coded values in scripts
* use properties
    - `gs.getProperty('property_name')`
* use messages for user-displayable values
    - `gs.getMessage('message_name')`
* use groups & roles

##### Counting Records
* Avoid using GlideRecord.getRowCount() when counting an undetermined number of records
    - Use GlideAggregate
