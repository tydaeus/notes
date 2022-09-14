# Git - Identifying Revisions
Git allows identifying specific revisions in a variety of different ways. The official reference is at: https://git-scm.com/docs/gitrevisions.

## SHA1
The full SHA1 hash allows uniquely identifying a commit within the repository it occurred in. Commits can also be identified by a truncated version of their hash (conventionally 7 characters) as long as the truncated version remains unique.

## HEAD
`HEAD` is a special identifier used to identify the active commit for the working environment. E.g. when you switch to another branch, `HEAD` points to the latest commit on that branch; if you checkout a specific revision, `HEAD` points to that revision (detached `HEAD` if this is a historical commit).

## Branch Name
The name of a branch can be used to identify the latest commit to that branch. Without further qualification, the local version of the branch is referenced; remote branches can be identified as `remotes/<remote name>/<branch name>`.

## `~` Operator
The `~` operator can be used to identify a commit prior to a specific commit; e.g. `HEAD~1` refers to the commit before head, `HEAD~2` refers to the commit before that, etc.

The `^` operator behaves similarly, but also provides an alternate syntax where each appended carat indicates one level higher; e.g. `HEAD^` is the same as `HEAD~1`, `HEAD^^` is the same as `HEAD~2`, and so on.

## `git rev-parse`
The `git rev-parse` command allows you to convert a non-SHA1 commit identifier to the unique SHA1 identifier for that commit. E.g. `git rev-parse HEAD~5` will give you the SHA1 for 5 commits prior to `HEAD`.

Use the `--short` option to retrieve the truncated SHA1, e.g. `git rev-parse --short HEAD~5`.