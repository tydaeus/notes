# Git Submodules
The git submodules feature allows you to use a project from within another project, by keeping one repo as a subdirectory of the other, keeping the commits separate.

The submodule remains tied to a specific version (commit) of the other repository.

## Setup
`git submodule add <gitUrl> [subdirName]`

If `subdirName` is not specified, the subproject will be added as a dir named after the repo.

This will create `.gitmodules` file to store the mapping to the submodule and the directory to hold it.

Although the OS will see the submodule dir as a plain directory, git treats it specially.

To properly display submodule-related info with `git diff`, you will need to add the `--submodule`.

Remember that the change will be local-only until it gets pushed, just like any other change.

## Retrieving Submodule Files

Adding a submodule does not automatically retrieve its files, and the default `git clone` operation will retrieve the metadata for the submodules but will not retrieve their file content by default.

Populate the submodule structure by running one of the following:
* Both:
    1. `git submodule init`
    2. `git submodule update`
* `git submodule update --init`
* `git submodule update --init --recursive` - will also recursively retrieve files for nested submodules

Use `git clone` with the `--recurse-submodules` command to automatically retrieve submodule files at clone time (recursing into nested submodules, etc.).

## Submodule Status
`git submodule` lists configured submodules and their current commit.

## Submodule Updating

### Updating Submodule Version and Files
To update the version of submodules and retrieve corresponding files, do one of:

Multi-Command:
1. `cd` into the submodule dir
2. `git fetch`
3. `git merge`

Single-Command:
* `git submodule update --remote [<submodulePath>]` - updates all submodules by default

Don't forget to `git push` your changes.

### Retrieving Submodule Updates from Server
When someone else has updated the submodule version, you'll need to make sure you get the updated version and files:

* After pull, run:
    + `git submodule update --init --recursive` - best to run with `--init` set so that any newly added submodules are retrieved, and `--recursive` set so that nested submodules also get updated.
* To combine with pull, run: `git pull --recurse-submodule` (will still need to do the submodule `--init` if new submodules added)


### Submodule Url Changed
If the submodule has changed to a different url, you will need to run `git submodule sync --recursive` to resolve the change before you'll be able to update.


## Making Changes within Submodules
By default, you will not be able to save changes within submodules because they operate in "detached HEAD" state. It is possible to check out a branch within a submodule and use it as a working copy; notes pending need.

## Switching Branches with Submodules
Ensure you include the `--recurse-submodule` option when changing branches, or else you'll need to update your submodules after the switch so that they contain the right files.


## Configuring Submodules

### Git Utility Configurations
As usual, the `--global` switch can be dropped from these commands to make the configuration specific to the current project.

* `git config --global diff.submodule log` - show submodule differences in `git diff`
* `git config --global status.submodulesummary 1` - show submodule summary in `git status`
* `git config --global submodule.recurse true` - use --recurse-submodules flag for all commands that support it, except for `clone`

### Configure Submodule Branch
`git config -f .gitmodules submodule.<moduleName>.branch <branchName>`

Drop the `-f` to make the change local-only, without storing it in the submodule config file.

You will need to update submodule files after setting this, to reflect the changes.

**Warning**: on its own, this only updates the configured branch that is pointed to, without updating the corresponding commit. You will also need to:
1. `cd` into the branch dir
2. `git switch` to the desired branch
3. `git fetch`
4. `git merge`
5. commit and push the resulting changes


## Removing a Submodule
`git rm <submodulePath>` (followed by commit and push, etc.)

This will remove the tracking data remove the working directory from the file system, but retain the git directory to make it possible to check out past commits without a fetch. To completely remove, delete `.git/modules/<submoduleName>`.


## Sources
* https://git-scm.com/book/en/v2/Git-Tools-Submodules
* https://git-scm.com/docs/gitsubmodules