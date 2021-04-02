# Git Initial Setup

## Configure User Settings
By default these will be specified for the current repo. Add `--global` to specify user-wide defaults.

User settings are stored in the user's home folder as `.gitconfig`.

* `git config user.name <user name>` - specify user's given name
* `git config user.email <user email>` - specify user's email address
    - **Important** - to prevent your email address from leaking, ensure that the "Keep my email address private" checkbox is checked in your github settings. Once you've done this, you will need to configure git to use your Github noreply address instead of your actual email address (mine is `5482721+tydaeus@users.noreply.github.com`), before you make any commits. For more info, see https://stackoverflow.com/a/44099011/2939139.
      - if you update an existing install in this way, you'll also need to use `git commit --amend --reset-author` on all repos that have unpushed commits predating the email address change, so that those commits are also associated with the noreply email.
* `git remote add origin <remote url>` - specify default repo location. If a github url, will be formatted as https://github.com/user-name/repo-name .

### Proxies
* `git config http.proxy=http://proxy.domain.com:PORT`
* `git config https.proxy=https://proxy.domain.com:PORT`

### Custom GitHub Certificates on Windows
If setting an older Git install up to connect to a Git repo that uses non-standard SSL certificates, you'll need to run `git config --global http.sslbackend schannel`.

This is now the default setting for Git for Windows installs, but the default does not get updated during Git updates.



## Create a New Repository on the Command Line

1. (optional) `touch README.md` - create the readme file
2. `git init` - init the current dir as a git repo
3. (optional) `git add README.md` - add the readme file to the repo
4. `git commit -m "first commit"` - commit changes to the local repo
5. `git remote add origin <repo url>` - designate default remote repository location
6. `git push -u origin master` - push changes to remote repo location as branch "master"

## Clone an Existing Repository
Cloning provides a local copy of the specified repository.

```
git clone <repo url>
```

Clone a specific branch:

```
git clone --single-branch --branch <branch name> <repo url>
```

Note that if you omit `--single-branch`, the entire repo will be checked out and the specified branch will be activated.