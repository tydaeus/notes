# Splunk Notes

## Splunk Enterprise Processing Components

### Indexer
Process incoming machine data, storing the results in indexes as events. Files are organized by age for search purposes.

### Search Head
Allows users to use Splunk search language to search index data. Distribute requests to indexes, then consolidate and enrich the results. Also provides dashboards, reports, and visualizations.

### Forwarder
Splunk Enterprise instances that consume data and forward it to the indexers. Minimal resource, little impact on performance. Usually reside on same machine as the desired data. Usually the primary way data is supplied for indexing.

## Deployments

### Single-instance deployment
One instance takes care of:

* Input
* Parsing
* Indexing
* Searching

Good for:

* proof of concept
* personal use
* learning
* small, department-sized environments

### Scaling
Scaling gets achieved by adding additional instances to act as forwarders, indexers, and search heads.

## Apps
Workspaces built to solve a specific use case. Defined by a user with an administrative role.

### Included Apps
* Home - Explore Splunk, Launch and Manage Apps, Find Docs, set a custom dashboard
* Search & Reporting - Allows power users to perform live searches

## Roles
Roles determine what a user is able to see, do, and interact with.

### Main Roles
* Administrator - install apps and create knowledge objects for all users
* Power - can create and share knowledge objects for users of an app and do realtime searches
* User - will only see their own knowledge objects and those shared with them

## Types of Data Input
Data gets added to the environment by an admin user.

### Getting Data In
1. Add Data Option from Home App
2. Choose one of the displayed options
    1. Upload - Onetime data load
    2. Monitor - Monitor files, dirs, http events, and data gathering scripts located on a Splunk instance. Windows events are also available in a Windows environment
    3. Forward - Accept data from forwarder at a remote location

#### Using Upload
1. Select the data
2. Select data source type - this is used to determine how to process the file
    1. can create a new source type
    2. can choose how to process dates
    3. can save and modify source types
    4. app context - tells which app to apply the source type to (system wide or specific apps)
3. Select host name - specify which machine the data came from
    * constant
    * regex
    * segment of filename
4. Select or create index to store the data in
    * single index can simplify searches
    * multiple indexes lets you restrict visibility, optimize results, retain for different intervals
5. Review settings
6. Submit - Data Gets Submitted and Indexed

#### Using Monitor
Similar to upload, but reads from files or ports

1. select source to monitor
    * Files & dirs
    * HTTP Event Collector
    * TCP/UDP port listening
    * scripts - any API, service, or DB
2. Continuous monitoring or one-time; whitelist or blacklist specific files
3. Select source type
4. define host name, select index, and app
5. Submit - data starts getting indexed

#### Forwarders - out of course scope


## Basic Search
The Search and Reporting App provides default interface for searching and analyzing data. It allows you to create:

* Knowledge Objects
* Reports
* Dashboards
* More

Enter search string into search bar, select time period, and click the magnifying glass.

Past searches can be viewed and re-run using the Search History tool.

Commands that create statistics and visualizations are called `transforming` commands. These are commands that transform event data into data tables.

### Wildcards

* `*` - allows for matching a partial word; e.g. `fail*` will match any word starting with "fail"
* `AND`, `OR`, and `NOT` (upper-case specifically) - allow boolean combinations; `AND` is implied by default
* `()` - Use parentheses to control order of evaluation
* `""` - search for phrase in quotes
* `\"` - escape quotes to search for literal quotes

## Using Fields
### Selecting Fields
Fields extracted during search are displayed in the `fields` sidebar on the left. `Interesting fields` are fields selected for having value in at least 20% of selected events.

Use the "all fields" or "more fields" links to show all fields.

### Searching Fields
Use $fieldName$operator$fieldValue syntax to search for matches on specific fields.

Field names are case sensitive, values are not.

#### Operators
* `=`, `!=` - numerical or string non/equality
* `>`, `>=`, `<`, `<=` - numerical comparison

Wildcards, boolean operators, and  parentheses can also be used when searching fields.

### Extracting New Fields
If a field you are interested in is not captured, use the `Extract New Fields` link displayed at the bottom of the `fields` sidebar.

This tool allows you to select a sample event, then use sample fields from it to generate field extraction rules in either regex or separator format. **Warning**: generated regex is likely to be positional, rather than based on text values. Be sure to verify that wildcards and specific text are used as appropriate.

After you have designated fields for extraction, these fields can be edited in future via `Settings > Fields > Field extractions`

#### Regex Capture Groups
Splunk field extraction uses custom capture group operators `(?P<FIELDNAME>REGEX)` to indicate when a field is being captured for extraction. For example, given input `MyField:12345`, you'd capture the numeric value of `MyField` as `my_captured_field` with regex: `MyField:(?P<my_captured_field>\d+)`. 



## Best Practices
### In Search
* Use time period to limit results returned
* Use more specific queries instead of more general queries
* Inclusion is better than exclusion
* apply filtering commands as early as possible

### Using Time
Preset time frames are included with Splunk. You can also specify specific intervals, including "now" to provide a real-time search effect. Advanced time frame settings can tailor intervals further.

Times can be specified within the search string by using the `earliest` and `latest` query params to specify endpoints. These endpoints can be specified using time abbreviations or with literal times in MM/dd/yyyy:hh:mm:ss format.

#### Time Abbreviations
* `s` - seconds; e.g. -30s would specify a time 30 seconds ago
* `m` - minutes
* `h` - hours
* `d` - days
* `w` - weeks
* `mon` - months
* `y` - years
* `@` - round down to nearest unit; e.g. -30m@h would be current time, -30 minutes, rounding back to the previous hour, so 9:37 -> 9:00

### Using Indexes
Segregating data into specific indexes can allow you to restrict your searches to a specific data set, making them more efficient.

Use the `index` query param to specify the index to search; e.g. `index=web`. Boolean operators and wildcards can be used when specifying index.

## SPL Fundamentals
SPL - the Splunk Search Language. Built from:

* Search Terms - what to search by
* Commands - what do with search results
* Functions - how to chart, compute, and evaluate results
* Arguments - what variables to apply to the functions
* Clauses - explain how we want results grouped or defined

A search is broken down into multiple search components. These are separated by pipes (`|`), indicating that the output from the previous component is being passed into the next component.

### Visual Syntax Tools
Autocompletion, parenthesis matching, and color coding are provided as you type into the search bar.

* Orange - boolean operators
* Blue - Commands
* Green - Command Arguments
* Purple - Functions

### Hotkeys
* `ctl` + `\` - display each pipe on separate lines

### Search Limitations
Left-to-right component pipeline is critical to functionality. Each component successively narrows the items available to operate on. A given component cannot operate on data that was removed by a previous component.

### Commands
#### The Fields Command
Include/exclude specific fields from results.

* `| fields FIELD1 FIELD2...` to include only the specified fields
* `| fields - FIELD1 FIELD2...` to exclude the specified fields

Field extraction is a costly part of the search process. Because field inclusion happens before extraction, it can improve performance. Field exclusion happens after extraction, so it affects displayed results only.

#### The Table Command
Retains searched data in a tabulated format, where specified fields are represented as columns and each record is represented as a row.

`| table FIELD1, FIELD2...`. Columns are displayed in the order listed.

#### The Rename Command
Renames a field. Multi-word phrases can be used as names when renaming, and must be wrapped in double-quotes.

`| rename FIELD1 as "FIELD NAME" FIELD2 as "FIELD NAME 2"...`

Once a field has been renamed, subsequent components must operate on its new name.

#### The Dedup Command
Remove duplicate events that share common values.

`| dedup FIELD1` removes events where FIELD1 has the same value.

`| dedup FIELD1 FIELD2...` removes events where the combination of FIELD1 and FIELD2 is the same.

#### The Sort Command
Sort results in ascending or descending order.

`| sort FIELD1 FIELD2...` sorts by FIELD1, then by FIELD2.

Use the `+` and `-` operators to set ascending or descending order, defaulting to ascending.

With a space between operator and field name, the operator controls the sort for all fields:
`| sort - FIELD1 FIELD2` will sort by FIELD1 in descending order, then by FIELD2 in descending order.

With the operator directly next to field name, the operator controls the sort only for the named field:
`| sort -FIELD1 FIELD2` will sort by FIELD1 in descending order, then by FIELD2 in ascending order.

Use the `limit` argument to limit the number of results displayed. `| sort FIELD1 limit=20` will only display the first 20 results, as listed in ascending order.

## Transforming Commands
Order search results into a data table that Splunk can use for statistical purposes. Allows you to transform search results into visualizations.

### The `Top` Command
Finds the most common values of a given field.

`|top FIELDNAME` will display count and percent for FIELDNAME's 10 most common values.

Use the `limit` int clause to specify how many top results should be displayed: `|top FIELDNAME limit=20` will display the top 20 results. Use `limit=0` to get top data for all values.

Add additional fieldnames to get combined top counts: `|top FIELD1 FIELD2` will display count and percent for the top 10 combinations of values for FIELD1 and FIELD2.

Use the `countfield` and `percentfield` string clauses to specify the titles for the count and percent columns. Use the `showcount` and `showperc` boolean clauses to show or hide the count and percent columns. Use `showother` (boolean) to toggle display of a row for results not within the limit, and `otherstr` (string) to specify the name for this row.

Use the `by` fieldname clause to show top results split by another field. `|top FIELD1 by FIELD2 limit=3` will show the top 3 FIELD2 values for each FIELD1 value.

### The `Rare` Command
Similar to the `top` command, the `rare` command shows the least common values for the specified field. All `top` clauses can be used with `rare`.

### `Stats` Command
Use `stats` to produce statistics for a search. `stats` works with functions to produce its results; multiple functions can be used within a single stats command, similar to specifying multiple columns for a table.

It appears that the `by` clause is best provided last when multiple stats are used, and applies to all stats.

#### The `count` Function
Returns number of events matching search criteria.

`|stats count` will return a count of all events.

Use the `as` clause to rename the displayed count column.

Provide a fieldname as an argument to the `count` function to count events where the named field is present: `|stats count(FIELD1)` will only count events where FIELD1 is present.

You can specify multiple comma-separated counts that return compatible results: `|stats count(FIELD1) as "FIELD1 Events", count as "Total Events"`.

Use the `by` clause to return a count by one or more (comma-separated) fields: `|stats count by FIELD1, FIELD2` will produce a count for each combination of values for FIELD1 and FIELD2.

#### The distinct count (`distinct_count` or `dc`) Function
Returns count of how many unique values exist for a given field.

`|stats distinct_count(FIELD1)` returns number of unique values present in FIELD1.

Distinct count can use the same clauses as count. The `by` clause is particularly useful.

#### The `sum` Function
Returns the sum of numerical values.

`|stats sum(FIELD1)` provides the total of numeric field FIELD1.

Use the `by` clause to split the sum up by another field.

#### The average (`avg`) Function
Returns the average of numerical values.

`|stats avg(FIELD1)` will return the average value in FIELD1.

Missing or misformatted values will be omitted from the calculation.

Use the `by` clause to average by a specified other field.

#### The `list` Function
List all values of a given field.

`|stats list(FIELD1)` will return all values of FIELD1. Again, particularly useful with the `by` clause.

#### The values (`values`) Function
List unique values of a given field.

`|stats values(FIELD1)` will return all unique values of FIELD1. Again, particularly useful with the `by` clause.

## Reports and Dashboards

### Reports
Select Save As > Report; enter name, description, and whether to use the Time Range Picker. Recommended: develop and use a naming convention.

Reports are accessible from the Reports tab.

Reports can be run as the report owner or as a user. By running as Owner, you can expose selected privileged info to unprivileged users.

Reports can be scheduled to run at timed intervals using the "Edit Schedule" option on the report.

Some reports can be accelerated. This stores the results in a smaller summary of data, that can be searched without searching the full index.

Quick reports can be generated using the fields sidebar from the search app. Select the field of interest and a desired statistic, and the data will be displayed in the visualization tab. This can then be saved as a report.

### Visualizations
Any search that returns statistical values can be viewed as a chart. Use the Visualization tab to view search results as a chart. Use the chart type drop down to select what type of chart you want to view, then customize further using the format menu.

Charts can be based on numbers, time, or location.

Visualizations are interactive. Visualizations can be saved as a report or as a dashboard panel.

### Dashboards
A dashboard is a collection of reports, compiled into a single view.

Start from a search that generates stats. Select the visualizations tab. Select type of visualization. Customize via format menu.

Save as Dashboard panel. The Panel can be added to a new Dashboard (creating this Dashboard in the process), or added to an existing Dashboard.

From the Dashboards view, use the edit menu to allow click and drag repositioning and resizing of panels.

Inputs can be added to the dashboard, then referenced via any inline searches on the dashboard.

## Pivot and Datasets

### Pivot
Pivot allows users to design reports in a user-friendly manner, without needing to formulate a search string.

`Data Models` - knowledge objects that provide the data structure that drives Pivots. Created by Admin and Power users. Data model is a framework, and pivot provide an interface into the data. Data models are made up of `Datasets`.

Data model is made up of data sets - smaller collections of data, defined for a specific purpose. Data model is represented as a table, with field names for columns and field values for cells.

Settings > Data Models. Click the `Pivot` link. Select which dataset to work with. These are organized into a hierarchy of increasingly narrow data, similar to using the `AND` boolean in the search language.

Within Pivot creation/editing, can select time range, split rows (which field to use); split columns.

Use filters to narrow data as desired.

Use pivot sidebar to visualize data. Can save as a report or add to a dashboard.

### Instant Pivot
For when lusers want to create a report, but a data model doesn't already exist. Instant pivot lets them work with the data without first creating a data model.

Enter a non-transforming command into search bar. A pivot button will become available in the Statistics and Visualizations tabs. Click this button, select fields for use as data model. Now we can create a Pivot from the selected data.

Because Pivot reports require a data model, saving an Instant Pivot will create a data model, and allow you to title the newly created model.

### Datasets
Datasets allow users to see a small slice of data. Use the datasets tab from Search & Reporting to view datasets. Some datasets will also link to summarized fields, providing data about each field name. Use the Explore menu to visualize with Pivot or Investigate in Search.

Visualizing with Pivot creates a new Pivot, allowing you to explore the data.

Investigate in Search opens the base search in the Search interface.

## Lookups
Allow you to add other fields and values that aren't included in the index data, by pairing fields with data from csv files, scripts, or geospatial data. E.g. tying readable data to id values.

After a lookup has been configured, you can use its field in search, and it will show up in the fields sidebar.

A lookup is categorized as a dataset.

To set up a lookup file:

1. Define a lookup table
2. Define the lookup
3. Optionally set the lookup to run automatically

Lookup field values are case-sensitive by default.

### AddLookup Table
Settings > Lookups > Add New Lookup table file
Select destination app.
Select file
Choose name for use on the server.
Use the lookup list to set permissions if other users should be able to use it.

Test lookup by entering `|inputlookup FILENAME` into the search bar. This will display the contents of the lookup file.

### Define Lookup
Settings > Lookups > Add New Lookup Definition
Select destination app
Give a name to the lookup
Select type
Select the lookup file (or other data specific to type)

Time-based lookup: if you have a field in the lookup table that represents time, use the "Configure time-based lookup" checkbox to specify how to interpret it.

Advanced options:

 * can define minimum and maximum matches, and a default value for cases where the minimum is not meant. (I'm guessing this is for cases when you're looking up against a count?)
 * can specify case sensitivity
 * batch index query can improve search performance with large lookup files by grouping index queries
 * match type - can set up non-exact matching
 * filter lookup - allows you to use SPL to filter the lookup results before returning them

### Lookup Command
Use the `lookup` command to access the defined lookup. `|lookup LOOKUPNAME LOOKUPFIELDNAME as DATAFIELDNAME` will lookup against the lookup named LOOKUPNAME, matching data in DATAFIELDNAME based on the lookup table column named LOOKUPFIELDNAME (this is the input field).

All fields except the input field will be returned as output fields by default; use the `OUTPUT` clause to choose which fields the lookup returns. This will overwrite existing fields of the same name, so you can use the `as` option to rename the output fields. E.g. `|lookup LOOKUPNAME LOOKUPFIELDNAME as DATAFIELDNAME, OUTPUT LOOKUPFIELDNAME as "Lookup Field Alias", LOOKUPFIELDNAME2 as "Lookup Field Alias 2"`. Use `OUTPUTNEW` instead to prevent overwriting existing fields.

### Automatic Lookup
1. Settings > Lookups > Add New Automatic Lookup
2. Select app
3. specify name
4. specify lookup table
5. choose what to apply the lookup to
6. specify lookup input field(s)
7. specify lookup output field(s)
8. Select whether to overwrite existing fields.

Now searches against the specified sourcetype will automatically apply the lookup.

### Additional Options
* Populate lookup with search results
* Define lookup based on external script or command
* Use Splunk DB Connect to lookup against external DB
* Use geospatial lookups to create queries that can be used to generate choropleth map visualizations
* Populate events with KV store fields

## Scheduled Reports and Alerts
Scheduled reports run on an interval and can trigger an action each time they run. Use for weekly or monthly reports, dashboards, or emailed reports.

Splunk tokens (delimited with `$`) can be used to add dynamic data when generating emails

### Creating Scheduled Reports
1. Start with a search
2. Save as Report
3. Provide a name, decline time range picker
4. Select schedule
5. Check schedule report
6. Select frequency
7. Select time range - relative to the schedule
8. Running concurrent scheduled reports, especially with the searches behind them, can be demanding; use the schedule priority and schedule window to help manage this.
    - priority allows for setting some reports to run at higher priority than others (admin only)
    - schedule window allows you to designate an acceptable time window when the report can be run. Do not use this if the report absolutely has to start at a specific time. Auto lets Splunk select the best time window in which to run the report.
9. Add Actions allows you to select what actions are triggered when the report is run.

### Managing Scheduled Reports
Use Settings > Searches, Reports, and Alerts to manage scheduled reports, or use the reports tab from Search and Reporting.

Enable report embedding to allow the report to be viewed on a web page; use provided iFrame html on the page. Anyone with access to the page will be able to see the report (does the page authenticate its request, or is Splunk providing this without any authentication). Report will not show data until the scheduled search is run. Once embedded, the report cannot be reconfigured.

### Alerts
Alerts are based on searches that run on an interval or in real-time. Alerts will notify you when the results of the search meet a defined condition. They are triggered when the search is completed.

Alerts can:

* be listed in the interface
* log events
* output to a lookup
* send to a telemetry endpoint
* trigger scripts
* send emails
* use a webhook
* run a custom alert

#### Creating Alerts
1. define a search
2. save as Alert
3. enter title
4. set permissions
5. choose scheduled or realtime
    * schedule lets you set a schedule and time range - predefined or chron expression
    * realtime will run the search continuously in the background - be careful with this!
6. Set a triggering threshhold - this allows you to confirm a problem before sending
7. Select whether to trigger just once (per trigger interval) or for each result (once is generally better, to avoid overwhelming recipients)
8. Select throttle to specify a duration to suppress alerts after triggering
9. add actions to be taken on the triggering condition
    - "Add to Triggered Alerts" will add the alert to the list of triggered alerts, and allow you to set a severity
    - "Log Event" will log the event to the Splunk deployment for indexing
    - "Output results to lookup" allows you to append or replace a lookup table with the results of the alert
    - "Output results to telemetry end point"
    - "run a script" will trigger a shell script - officially deprecated, use custom alert action instead
    - "send email"
    - "webhooks" let you perform an http action
    - custom actions can be added or created

#### Managing Alerts
Activity > Triggered alerts lets you view and manage alerts that were added to triggered alerts.
Alerts can be managed using the option from the Settings menu or the Alerts tab from the Search and Reporting app.
