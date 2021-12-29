# Common Git Commands

## access help (html formatted)

* `git --help`, or `git <cmd> --help`

## Setup
* `git init` initializes git on the current or specified dir (prereq to others)
* `git clone` retrieves a copy of the specified repo for you to work on (creates the containing folder)

## Saving Changes

* `git add` stages files for the next commit, e.g.:
    - `git add file.ext` stages file.ext
    - `git add .` stage everything within current directory
    - `git add . --all` stage everything within current directory, including deletions and moves (?)

* `git commit` saves to local repo
    - `git commit -m "<message>"` records `<message>` as commit message

* `git reset` unstages all staged files
* `git reset --hard` unstages all staged files **and** reverts any changes made. Does not delete newly created files.
    
## Pushing Changes to Repo

* `git remote` manipulate the location of named remote source, e.g:
    - `git remote add origin <url>` adds remote location at `<url>` with name 
      "origin"
      
* `git push` sends code to remote location, e.g:
    - `git push -u <remote> <branch>` updates `<remote>` version of `<branch>` (-u
      flag specifies that upstream references should be updated with successful
      changes and thus this version should be used by subsequent pulls)

## Retrieving Changes from Repo

* `git fetch` retrieve changes from remote repo without modifying local files (can be used to preview changes before merging)
* `git pull` incorporate changes from remote repo into current branch  (defaults to git fetch followed by git merge MERGE_HEAD)

## Detecting Changes

* `git status` shows what changes have been made since last commit, and which have been staged
* `git diff --cached|--staged` shows the staged changes against the HEAD

## Reverting Changes

### git checkout (most common)
Use `git checkout -- FILE` to replace FILE with the latest version from HEAD.

### git reset
`git reset` allows you to remove commits from the current branch. Recommended for use on private branches only.

For example, `git reset HEAD FILEPATH` will remove any commits to FILEPATH that are not part of HEAD. Note that this does not modify the file, just what changes to the file have been committed.

### git revert
`git revert <commit>...` makes a commit that reverts prior commit(s). Note that `git revert` cannot be restricted to a single file; it operates on the entire tree.

### git restore
`git restore <path>...` restores working files from HEAD.

`git restore --source=<tree> <path>...` restores working files based on specified source tree.



## Tags
Tags provide a way to bookmark the state of the repository at a specific commit, allowing for easier reference and comparison.

*Lightweight* tags simply mark a specific commit, while *annotated* tags include a name and a commit message. Annotated tags are generally preferable.

### Creating a Tag
On the latest commit: `git tag -a <tagname> -m <commit message>`

On a specific commit: `git tag -a <tagname> -m <commit message> <commit hash>`

### Listing Tags
`git tag [--list|-l]`

With glob matching: `git tag --list "<glob>"`

### Publish Tag(s) to Repository
Tags do not get published to the repository by default.

Push one tag: `git push <remotename> <tagname>`

Push all tags: `git push <remotename> --tags`

### Deleting a Tag
Locally: `git tag -d <tagname>`

Remotely: `git push <remotename> --delete <tagname>`



## Working with Branches
(adapted from https://stackoverflow.com/a/6591218/2939139)

Use `git branch` to list branches. The currently active branch will be indicated with an asterisk.

### Creating Branches
* `git branch <branchname>` to create a new local branch without switching to it (use `switch -c` or `checkout` to switch)
* `git switch -c <branchname>` to create a new local branch and switch to it
* `git checkout -b <branchname>` to create a new local branch and switch to it

Push this branch to your remote with `git push -u <remote> <branchname>`.

### Switching Branches
`git checkout <branchname>` to switch to another branch.

### Renaming (Moving) Branches
The `git branch -m` option renames (moves) branches.

* rename current branch: `git branch -m <newname>`
* rename a different branch: `git branch -m <oldname> <newname>`

### Deleting Branches
Delete a remote branch with `git push origin --delete <oldname>`.

