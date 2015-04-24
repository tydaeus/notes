# Development and Deployment

Both good technology and good process are required to get software from development to production

## Vocabulary

#### Development vs. Deployment

###### Development
* construction of the next unit of development
* individuals and teams often manage multiple work streams
* amenable to parallel, team-based processes
* where quality is engineered and assured

###### Deployment
* applying completed units of development to production
* execution of change management process
* not amenable to parallel execution
* where quality is confirmed by stakeholders


#### Updates and Versions

* **version** - tracks changes to a file
* **update**
    - prevents upgrades from changing a file
    - enables adding, updating, or deleting a file on another instance
* **update set**
    - holds arbitrary collection of updates
    - often retrieved and applied to other instances (*not always*)

#### Team Development
* allows collaboration between instances using a single **parent** or **hub** and many **child** instances
* allows for identification of and resolution of collisions on child instances
* supports code review on pushes to parent instance

#### Application
* a collection of related updates and optional demo data
* admins can
    - build applications
    - deploy aplications
    - install applications
    - customize applications

## Visualize Your Instance Graph
##### Every instance has its part to play in your success
(review of update process)
**In Fuji, do not complete an update set, then back out to global default**. 

## Independent Software Developer

At small scale, use Development -> Application Repository -> Test pipeline.

## Is Team Development Right for You?

### Questions to Consider
* Do you have multiple teams?
* Do you have a low tolerance for disruptions?
* At what level do you want to practice continuous integration?

### Needing Additional Instances != Needing Team Development

### Answering the question
#### A single Dev Instance
* you have
    - single team
    - good collaboration
    - simple timelines
* you get
    - simple process
    - continous integration

#### Team Development
* you have
    - multiple teams
    - collaboration challenges
    - complex timelines
* you get
    - within teams, continous integration
    - between teams, explicit integration and code review
    - boundaries at which you can enforce your process

### Why not deploy with Team Development?
###### (recommended to deploy via Update Sets)
* back out has very different consequences
* with update sets, backing out a commit on the target
    - does not affect the source instance
    - lets you commit again without re-retrieving
* with team development, backing out a push
    - wil undo its changes on source instance and all other children
    - requires you to re-implement those changes to push them again

## Update Sets are an Essential Tool
* update sets are **essential** for
    - customizing out-of-the-box applications
    - customizing purchased applications
* update sets are **useful** for
    - keeping track of *why* a change was made
    - exporting work in progress ahead of a clone
    - associating changes with SDLC artifacts
        + stories, problems, bugs, enhancements, etc.

#### Where Update Sets Shine
* update sets let you apply changes in bulk
* update sets help you force an instance to load needed file versions

## Deploying Applications
##### Install, customize. Rinse, repeat.

### Installation is Easy
* getting entitlement is slow
* pressing install button is fast

### App Customization is similar to OOTB Customization
* many records in a store app can be customized
* customizations are tracked in updates in update sets
* those update sets are specific to the scope of the application

### Two Deployment Techniques Supported
* through application repository
    - one-click publishing
    - one-click install
* through update sets
    - update set semantics
* pick one and stick with it
    - ServiceNow hopes you'll pick app repo
    - app repo will continue to evolve

