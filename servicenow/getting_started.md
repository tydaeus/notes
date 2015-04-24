# How to Start Using the ServiceNow Platform

## Types of Development

#### Citizen
* use serviceNow to solve own or group's problems

#### IT
* has applications to build for IT and getting demand from elsewhere

#### Professional
* build enterprise app and might sell to ServiceNow ecosystem

## Free Your Developers from the Easy Stuff

Empower users to create their own simple configuration and services.

* Service Creator makes it easy to create services
    - empower anyone to build lightweight custom request-fulfill apps

## Determine what Hard Stuff to Build

### What type of application is a good fit for ServiceNow?
Determining technical fit
* ideal candidates
    - form-based applications
    - workflow-based applications
        + approvals, multiple tasks, notifications, etc.
    - mobile access required
    - high reliability/availability required
    - benefits from organizational data
        + users, groups, locations, CIs, etc.
* poor candidates
    - graphically intensive applications
        + 3D modeling, games, etc

## get yourself up to speed

* wiki
    - product documentation
    - frequently updated
* training and certifications
* courses
* developer.servicenow.com
    - get an instance
    - get trained
    - join conversation
    - build applications

### Developer Program

* includes free access to some formerly-premium courses
* improved API documentation
* get a developer instance


## Spread the Love

### Selling Internally
##### How to Convince your Boss
* Platform
    - highly available
    - highly secure
    - reduced CapEx (minimal infrastructure costs)
* Consolidation
    - collaps disparate dev platforms into one
    - consolidated skill-set
    - consolidated admin to single system of record

### Demand Management
##### How to capture and analyze demand in the org
* Ideas
    - centrally capture potential demands
        + ideas, enhancements, feedback, etc.
    - convert Ideas to Demands
    - Idea module (new in Fuji) for capturing this data
* Demand Management
    - intelligently asses value using demand workbench
    - cost, risk, strategic alignment, ROI, and size provide comrehensive view
    - convert demands into actionable tasks

## Get it Done

### Pre-Fuji
* app suite was extensively cross-linked logically
* Apps had free reign to use and modify each other
* virtually impossible to install or modify apps without risking impact to other apps

### New Application Model (Post-Fuji)
* runtime application separation
* universally enforced namespacing
    - ensures unique resource naming within a given app
* installation, updating, and uninstallation
* public and private API definitions
* table level data access control

### Deployment
#### Applications Pre-Fuji and Customization for Fuji & Pre-Fuji
Update sets move from each environment to the next, and requires a order-sensitive, sensitive reconciliation process.

#### Application Deployment
1. publish application to application repository
2. install application to test instance for testing
3. install application to prod instance for production use

##### Versioning
* publicly visible version number used for user consumption
* ServiceNow maintains a hidden version number to ensure uniqueness

#### Application Management
System Definition > Applications
* develop applications
* install built or purchased
* update installed apps

## starting an application
1. applications page
2. select new
3. choose a starting point
    * start from scratch
    * create custom application
    * start from template
    * start from a service (through service creator)

## How do I Scale?
##### Moving to large scale development
* growing pains
    - multiple demands from multiple groups
    - growing dev team ( > 15 developers)
    - multiple dev teams in multiple locations
    - multiple releases on varying release cycles
    - low team cohesion
* what do I do?
    - SN recommends distributed development
    - multiple dev instances
    - let technology broker development
    - Team Development features
        + push updates to intermediary hub

### LifeCycle
##### SDLC plugin
* create and track releases natively
    - base sdlc provides structures for non-agile/waterfall dev enironments
    - products, enhancements, defects
* extend with SCRUM plugin
    - additional functionality to support agile dev environments
    - epics sprints stories, etc.
* closed loop dev, from ideation to realization

## Build vs. Buy
Store allows for browsing, trying, and buying application. Fuji adds limiting controls to what externally provisioned apps can do to system.
