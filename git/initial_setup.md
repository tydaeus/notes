# Git Initial Setup

## configure user settings
By default these will be specified for the current repo. Add `--global` to specify system-wide defaults.

* `git config user.name <user name>` - specify user's given name
* `git config user.email <user email>` - specify user's email address
* `git remote add origin <remote url>` - specify default repo location. If a github url, will be formatted as https://github.com/user-name/repo-name .

## create a new repository on the command line

1. (optional) `touch README.md` - create the readme file
2. `git init` - init the current dir as a git repo
3. (optional) `git add README.md` - add the readme file to the repo
4. `git commit -m "first commit"` - commit changes to the local repo
5. `git remote add origin <repo url>` - designate default remote repository location
6. `git push -u origin master` - push changes to remote repo location as branch "master"