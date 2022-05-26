# Viewing History in Git

## `git log`

### Viewing Commit Log

Use `git log` to view the history of commits with their time, user id, hash, and commit message.

#### Viewing Commit Details

Use `git log -p` to show the changes made in each commit in addition to the `git log` details.

Use `git log -p --follow FILENAME` to restrict the detailed logs to a specific file.

#### Revision History
The `rev-list` command lists revisions in the repository

``` bat
:: lists commit objects that are reachable from HEAD by following parent links in reverse chronological order
git rev-list HEAD

:: count commit objects that are reachable from HEAD by following parent links
git rev-list --count HEAD
```