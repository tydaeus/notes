* `git init` initializes git on the current or specified dir (prereq to others)

* `git add` stages files for the next commit, e.g.:
    - `git add file.ext` stages file.ext
    - `git add .` stage everything within current directory
    - `git add . --all` stage everything within current directory, including deletions and moves (?)

* `git commit` saves to local repo
    - `git commit -m "<message>"` records <message> as commit message
    
* `git remote` manipulate the location of named remote source, e.g:
    - `git remote add origin <url>` adds remote location at <url> with name 
      "origin"
      
* `git push` sends code to remote location, e.g:
    - `git push -u <remote> <branch>` updates <remote> version of <branch> (-u
      flag specifies that upstream references should be updated with successful
      changes and thus this version should be used by subsequent pulls)

* `git fetch` retrieve changes from remote repo
      
* `git pull` incorporate changes from remote repo into current branch 
  (defaults to git fetch followed by git merge MERGE_HEAD)

* `git status` shows what changes have been made since last commit, and which have been staged
* `git diff --cached|--staged` shows the stanged changes against the HEAD
