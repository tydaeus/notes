# Git Advanced Techniques

## Get a Full List of Contents
`git rev-list --objects` can be used to list the objects contained in the repo, while `git cat-file` can get information about files in the repo.

Get a full list of objects  in the repo along with their sha, type, and filepath via `git rev-list --objects --all | git cat-file --batch-check='%(objectname) %(objecttype) %(rest)'`; filter this to include only "blob" type objects to restrict this to files.

## Use `git-filter-repo` for Advnaced Repo Modification
`git-filter-repo` (https://github.com/newren/git-filter-repo) is a python script that allows for more advanced rapid modification of a git repo.

Use `--strip-blobs-with-ids` to remove a list of blobs by sha: `python git-filter-repo.py --strip-blobs-with-ids blobshafilepath`. This will remove the remotes (if present), so you will need to re-add them after.

## Use `git svn` to migrate a SVN repo to git
`git svn clone SVNURL --stdlayout OUTPUTDIR` - this will create the git clone, leaving it bound to the SVN repo only until you add a remote.