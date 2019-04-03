# Common SVN Operations

## Searching the Log

```
svn log [-v] [-r REVISION] [REPO_ _URL] --search [SEARCH_TERM]

# revision can be specified by rev#, name, or a date; can be a range

svn log -r 1729:HEAD
svn log -r {2001-12-04}:{2002-02-17}
svn log -r 1729:{2002-02-17}
```

Add the `--verbose` switch for more data, included names of changed files.

## Get Info About a Branch

```
svn info https://repo/wherever/project/trunk -r HEAD

# restrict to a specific data item:
svn info https://repo/wherever/project/trunk -r HEAD --show-item last-changed-revision
```

## List Contents of a Folder

```
svn list https://repo/wherever/project/trunk/folder
```

## Check if a Path Exists

```
svn ls https://repo/wherever/project/trunk/folder --depth empty
```

Exit code will be 0 if the directory exists, nonzero if not.
