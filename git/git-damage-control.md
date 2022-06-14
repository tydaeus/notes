# Git Damage Control
Notes on fixing common errors (and the possible ramifications).

## `git cherry-pick` - Picking a single commit to apply
The `git cherry-pick` command allows you to select a single commit and apply the changes from it to the working branch.

Use the `--no-commit` flag to skip the automatic commit so that you can review the change before committing.

``` bat
git cherry-pick c6ac2938f1fa83c7695729bf6affc6428bef7f4f
```

**Danger**: this applies the change in isolation from all prerequisite changes, so you run the risk of skipping critical prereq changes.


## `git reset`
Use `git reset --hard HEAD` to get rid of all uncommitted changes.

Use `git reset --hard <commit>` to get rid of all changes after `<commit>`.
