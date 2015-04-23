#Web Services Integration

##Table-Based Web Service APIs
* direct table access
* full CRUD ops on DB table
    - all enabled by default
* Access controls
* Scoped Applications and Custom Tables
    - scoped applications can designate independently whether they allow web service access
    - default deny
* No real transform capabilities
* Third party must understand and comply with ServiceNow scheme

## Import Set WEb Service APIs
* data flows through staging table
* only need to create staging records - system determines correct operation
* access controls
* mapping and transform capabilities
* coalescing capabilities
* third party can send data in own schema and format

## SOAP WEb Service APIs
* WSDL follows format:
    - /incident.do?WSDL
    - /u_my_custom_table.do?WSDL
* Direct table web service API
    - create
    - read
    - update
    - delete
* web service import set api
    - create (insert)
    - other CRUD functions available but not typically used

## RESTful history
* JSON Web Service
    - built early 2010
    - originally export processor
    - optional plugin
    - added function through URL params
    - inadvertently became the "REST API"
    - slow performance on larger tables
    - security concerns
* JSONv2 Web Service
    - introduced in Dublin
    - built in Java
    - based off JSON web service
    - installed by default
    - speed and security enhancement
    - primarily same interface as JSON web service
* Official REST API
    - intro'd in eureka
    - enabled by default
    - more traditional REST interface
    - requires "rest_service" role
    - access via `/api/now/table/tablename`

#### Supported HTTP Headers
* Accept
    - format we want back
    - text/xml (XML), or application/json (JSON)
* Content-type
    - format of data being sent
    - text/xml (XML), application/json (JSON)

#### Supported URL/Query Parameters
* sysparm_query
* sysparm_display_value
* sysparm_fields
* sysparm_view
* sysparm_limit
* sysparm_exclude_reference_link
* sysparm_read_replica_category
* sysparm_input_display_value
* key-value pairs

#### Built-in API Versioning
* URL Based Versioning
    - /api/now/v1/table/incident
* reference latest version
    - /api/now/table/incident

#### Misc Features & Reqs
* security
    - user ACLs still apply
    - rest_service role req'd for authenticating user
* debug option
    - inbound reqs can be writ to syslog

### Two Main APIs
* Table
    - direct table web service api
    - /api/now/table/{table_name}
    - POST, GET, PUT, PATCH, DELETE
* Import
    - Web service import set support
    - /api/now/import/{ws_import_set}


## Import Web Services
* fancy UI rendering of standard import set
* enable scripting and mapping around incoming data
* can view raw data that came in
* can decide whether or not to accept the data
* can blank out fields
* automatically determine if CREATE or UPDATE

## WEb Service Import Sets
* transform results returned to third party
    - import action that took place
    - table where record ended up
    - sys_id of new record
    - display value of new record
    - can send additional data if desired
* recommend using Correlation ID & Display

## Rest API Explorer
* covers all supported REST APIs in platform
* test interfaces in realtime
* auto generate script examples
* available in Fuji
