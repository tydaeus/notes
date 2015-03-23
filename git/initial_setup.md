access help (html formatted)
    - git --help, or git <cmd> --help

configure user settings; add --global to specify system-wide defaults
    - git config user.name <user name> // specify user's given name
    - git config user.email <user email> // specify user's email address

create a new repository on the command line
    - (optional) touch README.md
    - git init
    - (optional) git add README.md
    - git commit -m "first commit"
    - git remote add origin https://github.com/user/repo.git
    - git push -u origin master
    
push an existing repository from the command line
    - git remote add origin https://github.com/user/repo.git
    - git push -u origin master

