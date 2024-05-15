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
* `git reset --hard` unstages all staged files **and** discards any changes made. Does not delete newly created files.
    
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
* `git remote prune <remote name>` - perform a local cleanup to reflect changes to remote (e.g. after branches have been deleted on server)

## Detecting Changes

* `git status` shows what changes have been made since last commit, and which have been staged
* `git diff --cached|--staged` shows the staged changes against the HEAD
* `git show <commit>` shows the changes made by the specified commit

## Reverting Changes

### git checkout (most common)
Use `git checkout -- FILE` to replace FILE with the latest version from HEAD.

### git reset
`git reset` allows you to restore the current branch to the state it was in during a specified commit. Without any other specifiers, this results in a restore to HEAD state (unstaging all staged but uncommitted files). Recommended for use on private/local branches only.

`git reset <commit>` will reset the commit tree (by moving the HEAD pointer) to the state it was in at `<commit>` without making any file modification. This allows you to make additional revisions or change how you make the commit(s).

Adding the `--hard` switch specifies that the files should also be restored to the specified state (discarding any changes). Off-tree commits will persist until pruned.

### git revert
`git revert <commit>...` makes a commit that reverts prior commit(s). Note that `git revert` cannot be restricted to a single file; it operates on the entire tree.

#### Reverting merges
If reverting a merge, the mainline (parent to be reverted back to) must be specified. This is typically parent 1. `git revert <commit> --mainline 1`. Beware: GUI tools may make it unclear what the meaning of the selected parent is. Additionally, reverting a merge may not behave as expected, because while the file changes will be reverted, the git history will show the changes as having been merged in.

Best practices:
* if reversion is desired because an erronoeous change was merged in, prefer fixing the change in the source branch and then merging the change onto the target branch
* if you do revert a merge, you will then need to revert the revert if you desire the changes performed in the other branch to show up in the target branch
* if wrong-way merges have occurred, and especially if multiple have occurred, a `rebase` may be preferable (otherwise, will need to revert the changes on the incorrectly targeted branch, then revert the revert when it gets merged downstream so that the changes remain on their original branch)

For more detail than is likely helpful: https://github.com/git/git/blob/master/Documentation/howto/revert-a-faulty-merge.txt


### git restore
`git restore <path>...` restores working files from HEAD.

`git restore --source=<tree> <path>...` restores working files based on specified source tree.



## Merging
`git merge <branchname>` merges the named branch onto the current working branch, committing the branch. (Fails if conflicts are found)

`git merge <branchname> --no-commit --no-ff` applies the merge changes without committing them.


## Applying Changes without Commit aka Patching - `git apply`
`git apply` can be used to apply the output from `git diff` or `git show` to the working copy of file(s) within the local, without a commit.

Per documentation, this can be done by either recording the show/diff output to a file, then running `git apply <filepath>` or by piping the show/diff output directly into `git apply`.

Per documentation, if run within a subdir only changes within that subdir will be applied.

Personal experience has shown some difficulty applying `git diff` output (piped or from file) within PowerShell, with best luck piping `git show` output or using git bash to generate the patch instead.

**Example: Generate a Patch - Changes from current branch to apply to target branch:**

``` bash
git diff $targetBranch > $patchDestination
```

**Example: Generate a Patch - Changes from target branch to apply to current branch:**

``` bash
git diff HEAD..$targetBranch > $patchDestination
```

**Example: Apply patch to current branch**

``` bash
git apply $patchDestination
```


## Tags
Tags provide a way to bookmark the state of the repository at a specific commit, allowing for easier reference and comparison.

*Lightweight* tags simply mark a specific commit, while *annotated* tags include a name and a commit message. Annotated tags are generally preferable.

### Creating a Tag
On the latest commit: `git tag -a <tagname> -m <commit message>`

On a specific commit: `git tag -a <tagname> -m <commit message> <commit hash>`

### Listing Tags
* `git tag [--list|-l]` - all local
* `git ls-remote --tags <remotename>` - all remote (note that the normally-hidden tag data for the reference will be shown)

With glob matching: `git tag --list "<glob>"`

### Publish Tag(s) to Repository
Tags do not get published to the repository by default.

Push one tag: `git push <remotename> <tagname>`

Push all tags: `git push <remotename> --tags`

### Deleting a Tag
Locally: `git tag -d <tagname>`

Remotely: `git push <remotename> --delete <tagname>`


## Comparing Files

* `git diff branch1 branch2 -- filepath` changes in specified file between specified branches


## Working with Branches
(adapted from https://stackoverflow.com/a/6591218/2939139)

Use `git branch` to list branches. The currently active branch will be indicated with an asterisk.

### Creating Branches
* `git branch <branchname> [start-point]` to create a new local branch without switching to it (use `switch` subsequently to switch)
* `git switch -c <branchname>` to create a new local branch and switch to it
* `git checkout -b <branchname>` to create a new local branch and switch to it

Push this branch to your remote with `git push -u <remote> <branchname>`.

### Switching Branches
`git switch <branchname>` to switch to another branch.

### Renaming (Moving) Branches
The `git branch -m` option renames (moves) branches.

* rename current branch: `git branch -m <newname>`
* rename a different branch: `git branch -m <oldname> <newname>`

### Deleting Branches
* delete a local branch with `git branch --delete <branchname>`; must be switched to a different branch first
* delete a remote branch with `git push <remote> --delete <branchname>`; does not delete other users' local references

#### Remote Branches and Pruning
Remote branches may remain visible locally for some time after deletion. Initiate pruning of all remote branches with `git remote prune origin` or delete the reference to a specific one with `git branch -d -r origin/BRANCH`.

### Exploring Branches
Use ` git branch --contains <commit> -r` to find all branches containing the specified commit. (drop the `-r` for local-only)


## Stashing Changes
Stashing changes allows for setting aside in-progress changes and then retrieving them later, optionally to another branch.

* `git stash` (equiv to `git stash push`) to stash current changes and revert working state to HEAD.
* `git stash pop` to restore the latest stashed changes to the current branch and remove them from the stack.
* `git stash list` to list stashed changes.
* `git stash clear` - clear all stashed changes
* `git stash apply` - apply all stashed changes without removing them from the stack
* `git stash show` - show files in most recent stash
    - `git stash show -p` - show changes in most recent stash
    - `git stash show -p stash@{1}` - show changes in named stash
    - `git stash show -p 1` - show changes in numbered stash


## Listing Files - `git ls-files`

* `git ls-files`
    * `-c|--cached` - (default) list all tracked files
    * `-m|--modified` - files with an unstaged modification (deletions inluded)
    * `-o|--others` - untracked files
        - `-o --exclude-standard` to list untracked non-ignored files
    * `-d|--deleted` - unstaged deletions
    * `-i|--ignored` - ignored files; must also specify -c or -o and must specify one of the exclude flags to control the exclusion rule followed (typically `--exclude-standard`)