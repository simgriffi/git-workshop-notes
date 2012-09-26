### Slide 1
##Git Lunch and Learn
### Raymond Rusk and John Zhao

This mini-workshop is partially based on material presented by Matthew McCullough at the Victoria Java User Group Git Workshop held on August 29, 2012 and on other material presented in the following resources.

####Resources:
1. [Pro Git](http://git-scm.com/book) by Scott Chacon, Apress, 2009;
        free download in PDF, ePub formats)
2. [http://git-scm.com/documentation](http://git-scm.com/documentation) - many other Books, Videos
3. Wikipedia [Git (Software)](http://en.wikipedia.org/wiki/Git_%28software%29)
4. "Getting Started with Git" by Matthew McCullough, [DZone Refcardz](http://refcardz.dzone.com)
5. [Gitimmersion.com](http://gitimmersion.com) online Git lab

###Slide 2
##Git Overview

* Local repository is a full copy of the remote
* `git clone` fetches all branches and tags
* Almost all activities happen offline on local disk
* Offline activities are `git push`'ed to remotes

###Slide 3
##Initial Git Configuration

* To check your Git version and configuration:  

         $ git --version
         $ git config user.name
         $ git config user.email

* To set user-specific Git configuration (saved in `~/.gitconfig`):  

         $ git config --global user.name "Raymond Rusk"
         $ git config --global user.email "rarusk@cisco.com"
         $ git config --global color.ui "auto"

        On a Unix, OS X or Cygwin host, configure you favorite
        editor (needed for commit messages):  

         $ export VISUAL = /usr/bin/vim

        or provide a git-specific commit editor configuration  

         $ git config --global core.editor "vim"


### Slide 4
##Git Repo Creation

1. Create a local Git repo using `git init`  

     
     `$ mkdir mynewprj`  
     `$ cd mynewprj`  
     `$ git status # no git repo reported`  
     `$ git init`  
     `$ git status # git repo exists`

     or

     `$ git init mynewprj`  
     `$ cd mynewprj`  
     `$ git status`
        
2. Create a local Git repo using `git clone`:  


     `$ cd mynewdir`  
     `$ git clone git@github.com:rrusk/git-workshop-notes.git`  
     `$ cd git-workshop-notes`

### Slide 5
##Git Usage Overview

Using Git involves 3-stage thinking

1. Edit in working directory
2. Add working file to staging (index)
3. Commit staged file to repo  

See 3stage.pdf

### Slide 6
##Git Basic Usage

1) Adding and committing code  

       $ vim Notes.md
       $ git status  
       $ git add Notes.md
       $ git commit -m"First commit of workshop notes"

   Alternately, can add and commit in one step

       $ git commit -am"First commit with no staging"

   Files can be added to staging by name or by wildcard

       $ git add submodule1/PrimaryClass.java
       $ git add .
       $ git add *.java

   Can interactively add pieces from working file to staging using

       $ git add -p Notes.md

### Slide 6.1
##Git Basic Usage

Rather than using a sequential revision ID, Git marks
each commit with an SHA1 hash unique to the person committing the
changes, the folders, and the files comprising the changeset, allowing
commits to be independent of a central coordinating server.

For convenient, the current committed version is also called HEAD.
To view the SHA1 hash identifying head use

	$ git rev-parse HEAD

Earlier commits can be referenced by SHA1 or by using

	HEAD^, HEAD^^, HEAL-1, HEAD-2, etc.

### Slide 7
##Git Basic Usage (cont'd)  
2) Show file changes  

1. Show unstaged changes (diff working and staging areas)  

        $ git diff
2. Show staged changes (diff staging and repo code)  

        $ git diff --staged
3. Show changes in working and repo code  

        $ git diff HEAD
        $ git diff HEAD~5 # use 'git rev-parse HEAD~5' to see SHA1
4. Limiting diff output

  1. Show word changes rather than line differences

        `$ git diff --color-words`  
        `$ git diff --word-diff`

  2. Ignore whitespace  

        `$ git diff -w`

### Slide 8
##Git Basic Usage (cont'd)  
3)  Show history of commits  

1. Show all history  

     `$ git log`

2. Show all history with filenames  

     `$ git log --stat`

3. Show all history with patches  

     `$ git log -p`  
     `$ git log --patch`

4. Limit output  

     `$ git log --oneline # one line per commit`  
     `$ git log -n        # n = 1,2,3... commits`

5. Control log format  

     `$ git log --pretty=full  # or fuller, email, raw, format:<pattern>`
     `$ git log --graph --abbrev-commit --relative --decorate --oneline`

6. Limit output to added files  

     `$ git log --diff-filter=A`

### Slide 9
##Git Basic Usage (cont'd)  
4) Ignoring files

1. Via local .gitignore

        $ vim .gitignore

        #Add patterns to .gitignore, one per line
        *.log
        *.tmp
        target
        output/
        !wanted.log

2. Templates for many different development environments are
available at https://github.com/github/gitignore.git


### Slide 10
##Git Basic Usage (cont'd)  

5) Removing files

* Directly remove & stage deletion  

        $ git rm <FILENAME>

   Alternately, remove file at OS level and follow up with git add:

        $ rm <FILENAME>
        $ git add -u .


### Slide 11
##Git Basic Usage (cont'd)  

6) Moving files

* Directly move and stage

        $ git mv <FILENAME> <NEWFILENAME>

     Alternately, at OS level and follow up with git add:

        $ mv <FILENAME> <NEWFILENAME>
        $ git add -A .

* Git tracks file content by similarity index rather than file names.
        To see history of copies and renames use:

        $ git log --stat -C     # checks modified files as copy source
        $ git log --stat -C -C  # checks unmodified files also, expensive!


### Slide 12
##Git Basic Usage (cont'd)  
7) Aborting work in progress

* For a file that is changed but not staged

        $ git checkout <file>

* For a file that is changed and has been staged

        $ git reset HEAD <file> # removes changes from staging area
        $ git checkout <file>   # removes changes from working file

* To restore the working copy to the last committed state

        $ git reset --hard

* To restore the working copy to a specific commit

        $ git reset --hard <ref> # <ref> can be tag or SHA-1

  * Beware! When bad commits have already been pushed to a remote repository, resetting to a prior commit can confuse other users sharing the branch.
  * If you have made bad commits that have been tagged, reset removes them from your branch but not the repository.  They are still accessible through their tags and can be seen with `git log --all`.  To allow the commits to be garbage collected, remove the tags with `git tag -d <tagname>`.

* To restore just one file to its previous committed state

        $ git checkout -- Person.java

### Slide 12.1
##Git Basic Usage (cont'd)  
8) Check File Authoring using

        $ git blame filename.ext

  * True source of code shown with

        $ git blame filename.ext -C

### Slide 13
##More about Cloning URLs
1. File-based (could be network fileserver)

        $ git clone file://repos/project
        $ git clone /repos/project

2. git

        $ git clone git://server/project.git

3. ssh (read-only or read-write)

        $ git clone git+ssh://user@server:project.git
        $ git clone user@server:project.git

4. http

        $ git clone http://server/project.git
        $ git clone https://server/project.git

### Slides 14 and 15
##Branches
1. List local branches

        $ git branch

2. List remote branches (stored locally)

        $ git branch -r

        Remotes are just symbolic names
        The default name is origin if you've cloned
        If a remote branch is removed from an upstream
        repository, prune locally using

        $ git remote prune <REMOTENAME>

	$ git remote -v # verbose listing of remote branches

3. List all branches

        $ git branch -a

4. List upstream branches

        $ git ls-remote origin

5. Default branch name is master

        Nothing special about it.

6. Create a new local branch from HEAD

        $ git branch <BRANCHNAME>
        $ git branch <BRANCHNAME> HEAD

7. Create a branch from REF

        $ git branch <BRANCHNAME> <REF>

        Here <REF> can be the commit SHA1 of some commit node.

8. Switch to another branch

        $ git checkout <BRANCHNAME>

9. Create and switch to another branch

        $ git checkout -b <NEWBRANCH> <FROMBRANCH>

10. Git branches are cheap (20 bytes)

        Create branches for experiments
        Delete failed experiments

### Slide 17
## Commit/Push/Pull

1. Commit

        $ git commit
        Saves code snapshot
        Commits to local branch
        Operates on local disk only.

2. Push

        $ git push <remote>
        Send code to an upstream server
        Update remote branches

3. Pull

        $ git pull <remote>
        Retrieve upstream objects
        Update remote branch (stored locally)
        Merge changes into local branch
        Commit the merge to the local branch

### Slide 18
## Aliases

Examples:

        $ git config --global alias.s "status -u -s"
        $ git config --global alias.l "log --stat -C"
        $ git config --global alias.br "branch -a"
        $ git config --global alias.cns "commit -a"
        $ git config --global alias.sync "!git pull && git push"

or edit `$HOME/.gitconfig` directly.  For instance,

        [alias]
          co = checkout
          ci = commit
          st = status
          br = branch
          lg = log --pretty=format:\"%h %ad | %s%d [%an]\" --graph --date=short


### Slide 19 and 20
## Stashes

        Super quick branch
        Local-only branch
        Numbered branch, stack based implementation
        Push, Peek and Pop operations plus direct entry access

1. Stashing pending changes

        $ git stash
        $ git stash save "<Message>"

2. List your stashes

        $ git stash list

3. Merge and delete the latest stash

        $ git stash pop
        $ git stash pop stash@{0}

4. Merge and keep the latest stash

        $ git stash apply

5. Convert a stash to a branch

        $ git stash branch workinprogress 
        $ git stash branch <newbr> stash@{3}

6. Relocating commits with stash

        $ git stash
        $ git checkout finalbranch
        $ git stash apply
        $ git commit


### Slide 21 and 22
## Tagging (cheap)

        Reference, annotated and signed tag types

1. Reference tags
   1. Tag head

        $ git tag <TAGNAME>
   2. Tag an existing ref

        $ git tag <TAGNAME> <REF>

2. Annotated tags
   1. Tag head with annotated tag

        $ git tag -a <TAGNAME>

3. Signed tags
   1. Tag head with signed tag

        $ git tag -s <TAGNAME>

4. List know tags

        $ git tag

5. Remove a tag

	$ git tag -d <TAGNAME>

6. Show a tag's content

        $ git show tag
        $ git show <TAGNAME>

8. Push tags

   Tags don't push by default  
   Push all tags

        $ git push <remote> <tag>
        $ git push --tags

8. Fetch tags

        Tags do fetch by default

### Slide 23
## Merging

1. Merging a feature branch into master branch

        $ git checkout master
        $ git merge <featurebranch>

2.  Octopus merge

        $ git checkout master
        $ git merge <fb1> <fb2> <fb3>

3.  Subtree merge (useful if multiple repos are blended together into a single repo)

        $ git checkout master
        $ git merge -s subtree <fb1>

### Slide 24
## Rebasing

1. Retrieve upstream changes and relocate your
        local changes to the end

        $ git pull --rebase

        Same as
        $ git checkout master
        $ git rebase origin/master

2. Rebase feature branch on master

        $ git checkout <freaturebranch>
        $ git rebase master

3. Warning: Do not rebase your code once it has already
  been committed to remote repositories!

### Slide 25 and 26
## Clean

Purges untracked files.  Leaves ignored and tracked files alone.

1. Dry-run remove files

        $ git clean -n
 
2. Dry-run remove files, directories

        $ git clean -nd

3. Remove untracked files.

        $ git clean -f

4. Remove untracked files, untracked directories.

        $ git clean -fd

5. Remove untracked files plus ignored files.

        $ git clean -xf

6. Remove ignored files and ignored directories

        $ git clean -xdf

### Slide 27
## Revert commits
1. Revert a single commit (will remain visible in branch history)

        $ git revert <ref> # use HEAD as <ref> for most recent commit

2. Revert a range of commits
        (<ref1>, <ref2> must be in right order)

        $ git revert <ref1>..<ref2>

### Slide 28
## Amend commits
1. Modify a bad commit message (in HEAD commit)

        $ git commit --amend -m "Much better commit message"

2. Add missing file

        $ git add <missingfile>
        $ git commit --amend


### Slide 29
## Git-svn

1. Clone one branch

        $ git svn clone <svnurl>

2. Clone all branches, tags

        $ git svn clone --stdlayout <svnurl>

  svn2git provides an alternate conversion tool
        that converts svn tags to Git tags.

3. Updating

        $ git svn rebase

4. Send new changes

        $ git svn dcommit

### Slide 30
## Fetching/Pulling

1. Git fetch commands do not merge

	$ git fetch

2. Git pull commands merge by default

        $ git pull

3. Git pull with rebase

        $ git pull --rebase

   To avoid typing and risky mistakes use

        $ git config branch.autosetuprebase always

### Slide 31
## Pushing configuration

1. Push all branches having the same name locally
        and remotely (the default)

        $ git config --global push.default matching

2. Do not push anything

        $ git config --global push.default nothing

3. Push current branch to what it is tracking

        $ git config --global push.default upstream

4. Push current to branch of the same name

        $ git config --global push.default current

### Slide 32
## Cherry pick merging

   Merge in just one commit (not entire branch)

        $ git cherry-pick a5b2ee
  
