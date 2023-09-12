# Viewing History in Git

## `git log`

### Viewing Commit Log

Use `git log [commit1...]` to view the history of commits with their time, user id, hash, and commit message.

`git log <commit1> <commit2> ^<commit3>` combines the commits that can be reached from commit1 and commit2, then subtracts those reachable from commit 3

`git log <commit1>..<commit2>` is an alternative way to write `git log ^<commit1> <commit2>`.

`git log <commit2>...<commit1>` displays "symmetrical difference" between commit1 and commit2 -- what commits are in either branch but not both.

Use cases:

* What's in branch1 that hasn't been merged to branch2? `git log branch1..branch2` or `git log ^branch1 branch2`.


#### Viewing Commit Details

* `--patch`, `-p`, or `-u` - show patching data (specific changes made)
* `--name-only` - show the names of the files changed
    - **Warning**: this switch may negate other switches without warning; e.g. the `--patch` switch will not show patch diffs when combined
* `--name-status` - filename and status of files changed
* `--stat` - abbreviated pathname and status

#### File History
`git log -- FILEPATH` will display the active history for a given file. Use `git log --follow FILEPATH` to show the full logs for the file, including those that remain in the tree but longer active (e.g. discarded during a merge).



## `git rev-list` - revision history
The `rev-list` command lists revisions in the repository

``` bat
:: lists commit objects that are reachable from HEAD by following parent links in reverse chronological order
git rev-list HEAD

:: count commit objects that are reachable from HEAD by following parent links
git rev-list --count HEAD
```

`--parents` switch shows commit parents as well -- very useful for tracing history.

## `git diff`
`git diff` shows the specific file changes from a commit, similar to using `git log -p`.

**With merges**: By default, `git diff` will only display diffs against the first parent. This works fine with normal commits, but merges have 2 or more parents. You will need to use `git diff` against each parent (e.g. `git diff <Commit>^1` and `git diff <Commit>^2` for parent 1 and parent 2) to see the full list of changes. Note that the first parent is always the current branch for the developer who made the merge, while the second (and later) parents represent the branches being merged in.

## `git show`
`git show` is a general-purpose command to show specific git objects. When referencing commits, it shows information about that commit.

`git show <commit>` - shows information about a commit, including hash, time, and author. The documentation implies `-p` must be specified to show differences as a patch, but this appears to be the current default.

Use `-m` if viewing a merge commit; otherwise only differences along the first parent will be shown.