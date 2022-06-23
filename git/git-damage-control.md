# Git Damage Control
Notes on fixing common errors (and the possible ramifications).

## `git cherry-pick` - Picking a single commit to apply
The `git cherry-pick` command allows you to select a single commit and apply the changes from it to the working branch.

Use the `--no-commit` flag to skip the automatic commit so that you can review the change before committing.

``` bat
git cherry-pick c6ac2938f1fa83c7695729bf6affc6428bef7f4f
```

**Danger**: this applies the change in isolation from all prerequisite changes, so you run the risk of skipping critical prereq changes.

## Reverting a single file
Use `git checkout <commit ID> -- <local filepath>` to restore the file at `<local filepath>` to the state it was at in `<commit ID>`.


## `git reset`
Use `git reset --hard HEAD` to get rid of all uncommitted changes.

Use `git reset --hard <commit>` to get rid of all changes after `<commit>`.

## `git rebase`
The `git rebase` command is a very powerful and dangerous command that allows reconstructing a branch. It is one of the most powerful tools for correcting history issues, but also one of the most dangerous.


**Danger**: rebasing edits history, which can have unexpected effects, especially if the branch being rebased has already been merged onto another branch or is in use by other developers. Except in extreme cases, rebasing should be performed on a local branch -- either before it becomes available to others, or by creating a new local branch based on the problem branch. Once the rebase has been completed, the branch can then be published and used in place of the problem branch.

See also: https://thoughtbot.com/blog/git-interactive-rebase-squash-amend-rewriting-history
