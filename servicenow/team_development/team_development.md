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

