# Team Development - Beyond Update Sets

#### Why use team development?

##### Problems

* more than one developer/admin working on more than one project
    - separate updates track changes separately, but only the latest change is active
* different apps may include common customizations
    - these apps become dependent upon each other

##### Solution via Team Development

* Team Development provides an instance architecture and functionality to support simultaneous customization of elements, with abillity to merge customizations later
    - Functionally equivalent to branching in trad'l source code control
        + Child instances act as separate branches
        + Parent/hub instance acts as master branch
        + Children pull from parent to stay up-to-date
        + Children push to parent, which requires conflict resolution
* small fixes and large dev efforts can be done concurrently
* applications can be published to the app repository prior to installation in production

## Using Team Development

#### Connecting Instances and Reconciliation

* done once, after instances provisioned and before team development begins
* child is configured to reference Parent
    - team development > remote instances > new
* chid instances can also be configured to reference peers
* reconciliation establishes baseline of local changes compared to parent

#### Versions
* Team development functions on versions
    - version table tracks changes to customized record over time so admins can compare or revert to specific versions
    - pull operation queries versions from parent and loads them into the child
    - both current and historical versions pulled, providing change history
* Local changes shows changes on child relative to parent
    - changing parents reconciles child changes to new parent
    - local changes are queued, and pushed to parent when ready

#### Pulling
* retrieves changes from parent and applies them to parent
* creates collisions when a customization has been changed in both parent and child
    - collision customization not applied until collision resolved
    - user resolves collision by accepting
        + remote customization
        + local customization
    - accepting remote cause remote customization to become current
    - manual merge by editing local customization to include code from remote, then accepting local
* Pull and resolution of collisions is required before pushing

#### Pushing
* user selects local changes and selects queue for push
    - only local changes that are different are available
* before items can be pushed, there can be no pullable items, and collisions must be resolved
* clicking pull, user must speiciy name of update set to place pushed changes
    - update set will be created on parent if it does not exist
* clicking push changes pushes changes to parent instances
    - changes are loaded and committed and become available for pull to other children
* local update set can be used to organize changes
    - they are "change lists"
    - can assist in sorting and filtering local changes list on team dash
* other wasy to filter
    - user (updated or created)
    - application set
    - date
* recommended filter
    - using application sets
* a push results in a completed Update Set in parent instance
    - records who pushed what from where
    - update sets can be merged in parent to reduce clutter
    - functionality same as trad'l update sets
* not forced to use only Team Development

## Additional Team Development Features

#### Exclusion Policy & Ignoring Changes
* in a non-prod instance, may have configurations that should never be pushed
    - can ignore a change
    - can create an exclusion policy

##### Ignoring a change
* mark a local change as ignored and it will not push
* good for one-off changes

##### Exclusion Policy
* a query specs which records to exclude
* can specify whether exclusion applies to pushes, pulls, or both
    - can only spec table if pull involved

#### Code Review
* optional feature to provide code review checkpoint before pushes consumed by parent
* enabled on parent
* reviewers can accept or reject
    - submitters can cancel pushes that are awaiting code review
* all pushes and pulls blocked while code reviews are pending 
* allows dev team to monitor what is being pushed

#### Comparing Peer Instances
* 2 peer instances can be compared, and local changes cna be applied from one to the other without pushing
    - define peer instances as remote instance
    - team development > team dashboard > compare to...
        + select remote instance
    - creates instance coparison record with related lists of local changes
* can be useful in reviewing changes other teams are making and identifying potential future collisions

## Best Practices

* understand technical deps between releases
    - partition work across teams and child instances to minimize collisions
* use team dev on the DEV portion of stack and update sets in higher environments
* ensure releases are identified before dev work begins, and that order of releases established when there are overlapping customizations
    - this drives who pushes to DEV first, and which children should pull
    - needs to be made with input from dev team (they know what will be customized)
* for multiple developer releases, nominate a developer to manage aspects of update set lifecycle
    - creates, pushes, and merges
    - communicates current update set to rest of team
    - reviews update sets and commmunicate pushes
    - documents deployment process
        + order of applying update sets, execution, and data to load
* when possible, favor shorter release cycles
    - reduces quantity of in-flight work, ensuring better match betw prod and dev
    - simplifies work associated with re-cloning dev stack
* always review updates in update set before transferring
    - look for updates associated with other dev efforts and updates associated with testing
    - watch for sys properties and integration end-points changes
        + e.g. sys_property change that directs email to test acct
    - move updates to scrap update set rather than deleting update
* limit who has admin on development stack -- use impersonation
    - ensures unintended customizations not made by non-developers
* limit direct customizations in DEV HUB -- be judicious
    - can be convenient way to provide change for all children, but can disruptive
        + you cannot verify feature before it's consumed by children
