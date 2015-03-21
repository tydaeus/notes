#Working with git remotes
Git remotes allow multiple different repositories and their branches to be associated with one project. The following
commands interact with remotes:

##managing remotes
The remote command allows you to manage the list of remotes

* `git remote` retrieves the list of remotes currently defined
    * `-v` displays associated urls
* `git remote add <REMOTENAME> <URL>` adds a new remote named `<REMOTENAME>` pointing to `<URL>`

##pushing to remotes

* `git push <REMOTENAME>` sends all matching branches that have the same names as remote branches
* `git push <REMOTENAME> <BRANCHNAME>` pushes local changes to branch `<BRANCHNAME>` of `<REMOTENAME>`
* `git push <REMOTENAME> <BRANCHNAME>:<REMOTEBRANCH>` pushes local changes to branch `<REMOTEBRANCH>` of `<REMOTENAME>`
* `git push <REMOTENAME> :<REMOTEBRANCH>` deletes `<REMOTEBRANCH>`
* tags can be managed by using `<TAGNAME>` in place of `<REMOTENAME>`
    * `--tags` pushes all tags

##remotes and forks
A cloned repo will have its standard remotes pointing to the cloner's server. To allow updating from and pull requests
that reference the original server, you must specify that server's remote url. By convention, this remote is called
**upstream**. Set this remote via:

    git remote set-url upstream  <THEIR_REMOTE_URL>
