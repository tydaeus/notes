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

The `-p`, `-u`, or `--patch` flag causes `git log` to output patching data.

Use `git log -p` to show the changes made in each commit in addition to the `git log` details.

#### File History
`git log -- FILEPATH` will display the active history for a given file. Use `git log --follow FILEPATH` to show the full logs for the file, including those that remain in the tree but longer active (e.g. discarded during a merge).

#### Revision History
The `rev-list` command lists revisions in the repository

``` bat
:: lists commit objects that are reachable from HEAD by following parent links in reverse chronological order
git rev-list HEAD

:: count commit objects that are reachable from HEAD by following parent links
git rev-list --count HEAD
```

## `git show`
`git show` is a general-purpose command to show specific git objects. When referencing commits, it shows information about that commit.

`git show <commit>` - shows information about a commit, including hash, time, and author. The documentation implies `-p` must be specified to show differences as a patch, but this appears to be the current default.

Use `-m` if viewing a merge commit; otherwise only differences along the first parent will be shown.