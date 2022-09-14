# Git Damage Control
Notes on fixing common errors (and the possible ramifications).

## `git cherry-pick` - Picking a single commit to apply
The `git cherry-pick` command allows you to select a single commit and apply the changes from it to the working branch.

Use the `--no-commit` flag to skip the automatic commit so that you can review the change before committing.

Use the `-x` flag to append the original commit's information (recommended if cherry-picking a publicly visible commit).

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

See also: https://thoughtbot.com/blog/git-interactive-rebase-squash-amend-rewriting-history, https://jwiegley.github.io/git-from-the-bottom-up/1-Repository/8-interactive-rebasing.html

### `rebase` current branch interactively
Rebasing the current branch interactively is generally the simplest option, allowing you to edit the current branch's history. 

1. run `rebase -i <commit> -i`
2. your configured text editor will open a file, listing the operations `rebase` will perform
    * this starts with all commits being `pick`ed in their original order
3. modify the text file to indicate the desired sequence of changes
    * operations will be performed in the order they're listed
    * `pick` will result in the commit being performed as-is
    * `reword` allows modifying the commit message before performing the commit as-is
    * `squash` combines the commit with the previous commit
    * `edit` allows editing the content of the commit
    * `drop` or deleting (or commenting) the commit from the listing results in the commit being removed from the branch
4. save and close the file when done editing to perform the commands (*warning*: 'Save As' will register the same as close)
5. if there are any merge conflicts resulting from the revised operations, you will need to address them, then run `rebase --continue`