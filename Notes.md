### Slide 1
##Git Lunch and Learn
### Raymond Rusk and John Zhao

This tutorial contains some material presented by Matthew McCullough at the Victoria Java User Group Git Workshop held on August 29, 2012.

####Resources:
1. [Pro Git](http://git-scm.com/book) by Scott Chacon, Apress, 2009;
	free download in PDF, ePub formats)
2. http://git-scm.com/documentation (many other Books, Videos)
3. Wikipedia [Git (Software)](http://en.wikipedia.org/wiki/Git_%28software%29)

###Slide 2
##Git Overview

* Local repository is a full copy of the remote
* `git clone` fetches all branches and tags
* Almost all activities happen offline on local disk
* Offline activities are `git push`'ed to remotes

###Slide 3
##Initial Git Configuration

* To check your Git version and configuration:  
         `$ git --version`  
         `$ git config user.name`  
         `$ git config user.email`

* To set user-specific Git configuration (saved in `~/.gitconfig`):

	`$ git config --global user.name "Raymond Rusk"`  
	`$ git config --global user.email "rarusk@cisco.com"`  

	On a Unix, OS X or Cygwin host, configure you favorite
	editor (needed for commit messages):  
	`$ export VISUAL = /usr/bin/vim`  
	or provide a git-specific commit editor configuration  
	`$ git config --global core.editor "vim"`:


### Slide 4
##Git Repo Creation

1. Create a local Git repo using `git init`:  
     `$ mkdir mynewdir`  
     `$ cd mynewdir`  
     `$ git status # no git repo reported`  
     `$ git init`  
     `$ git status # git repo exists`
	
2. Create a local Git repo using "git clone":  
     `$ cd mynewdir`  
     `$ git clone git@github.com:rrusk/git-workshop-notes.git`  
     `$ cd git-workshop-notes`

### Slide 5
##Git Usage Overview
Using Git involves 3-stage thinking

1. Edit in working directory
2. Add working file to staging
3. Commit staged file to repo  

See 3stage.pdf

### Slide 6
##Git Basic Usage
1. Adding and committing code  
    `$ vim Notes.md`  
    `$ git status`  
    `$ git add Notes.md`  
    `$ git commit -m"First commit of workshop notes"`  

   Alternately, can add and commit in one step  
    `$ git commit -am"First commit with no staging"`
