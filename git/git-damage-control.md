# Git Damage Control
Notes on fixing common errors (and the possible ramifications).

## Tracing Issues
`git log --graph` attempts to show the commit log graphically, but this becomes difficult to read as complexity increases.

`git rev-list --parents`, ideally with a `-n` constraint or date constraint through `--since=`, `--after=`, `--until=`, `--before=`, `--max-age=`, `--min-age=` can be used to help with understanding the tree strucuture precisely. Add `--chery-mark` to mark equivalent commits (as per cherry-picking or manual transfer) with an `=`, or `--cherry-pick` to omit equivalent commits.

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
Rebasing the current branch interactively is generally the simplest option, allowing you to edit the current branch's history. If the problem branch is already present on the server, this should be performed on a new local branch, and processes should be switched to using the new branch after fixes are complete (delete the old branch, after tagging for posterity if desired)

1. run `git rebase -i <commit>`, where `<commit>` is the last good commit
2. your configured text editor will open a file, listing the operations `rebase` will perform
    * this starts with all commits since `<commit>` being `pick`ed in their original order
3. modify the text file to indicate the desired sequence of changes
    * operations will be performed in the order they're listed
    * `pick` will result in the commit being performed as-is
    * `reword` allows modifying the commit message before performing the commit as-is
    * `squash` combines the commit with the previous commit
    * `edit` allows editing the content of the commit
    * `drop` or deleting (or commenting) the commit from the listing results in the commit being removed from the branch
4. save and close the file when done editing to perform the commands (*warning*: 'Save As' will register the same as close)
5. if there are any merge conflicts resulting from the revised operations, you will need to address them, then run `rebase --continue`


## .gitattributes and Line Endings
After a .gitattributes file has been added or files have been merged from a repo without .gitattributes to one with, git will sporadically report unmodified files as having changes made to their line endings. This is because .gitattributes processing changes the way files' line endings are stored internally, so all files stored prior to the change will need to be re-stored, but git only notices the difference during certain operations.

Files flagged as changed this way cannot be restored through the standard `git restore` or `git reset` operations.

### Removing the Changes
Once all desired changes have been committed, run the following commands to clear out the local status.

```
git rm --cached -r .
git reset --hard
```

Note that this does not resolve the issue, just hides it until the next time git notices the line ending issue.

### Renormalize Line Endings
To correct all line endings per the .gitattributes configuration:

``` sh
# from the repository root
git add --renormalize .
```

The resulting changes will need to be committed and pushed.