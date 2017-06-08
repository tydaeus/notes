access help (html formatted)

* `git --help`, or `git <cmd> --help`

configure user settings; add --global to specify system-wide defaults

* git config user.name <user name> // specify user's given name
* git config user.email <user email> // specify user's email address

create a new repository on the command line

1. (optional) touch README.md
2. git init
3. (optional) git add README.md
4. git commit -m "first commit"
5. git remote add origin https://github.com/user/repo.git
6. git push -u origin master
    
push an existing repository from the command line

1. git remote add origin https://github.com/user/repo.git
2. git push -u origin master

