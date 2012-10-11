#Git Lunch and Learn
### Raymond Rusk and John Zhao

This [mini-workshop](https://github.com/rrusk/git-workshop-notes) is partially based on material presented by Matthew McCullough at the Victoria Java User Group Git Workshop held on August 29, 2012 supplemented by material from the following resources.

####Resources:
1. [Pro Git](http://git-scm.com/book) by Scott Chacon, Apress, 2009  
        (free download in PDF, ePub formats)
2. [http://git-scm.com/documentation](http://git-scm.com/documentation) - many other Books, Videos
3. Wikipedia [Git (Software)](http://en.wikipedia.org/wiki/Git_%28software%29)
4. "Getting Started with Git" by Matthew McCullough, [DZone Refcardz](http://refcardz.dzone.com)
5. [Gitimmersion.com](http://gitimmersion.com) online Git lab
6. [Mastering Git Basics](http://vimeo.com/17118008) - Video tutorial on git basics with Github's co-founder Tom Preston-Werner

##Git Overview

* Local repository is a full copy of the remote
* `git clone` fetches all branches and tags
* Almost all activities happen offline on local disk
* Offline activities are `git push`'ed to remotes

##Installing Git

* Download _command-line git_ from [http://git-scm.com/download](http://git-scm.com/download)  
  It is included in most Linux distributions and is available via  
  the Cygwin package-manager [seteup.exe](http://cygwin.com/setup.exe).  
  The "official" Windows command-line version is available at [mysysgit.github.com](http://msysgit.github.com).

* Several GUI clients exist.  Git comes with 'gitk' for viewing commit history.  
  Window's TortoiseSVN users might like [TortoiseGit](http://code.google.com/p/tortoisegit).

* Also, there is a Git plugin for Eclipse, [EGit](http://www.eclipse.org/egit/), and Git  
support in Netbeans 7.1/7.2 [http://netbeans.org/kb/docs/ide/git.html](http://netbeans.org/kb/docs/ide/git.html)

##Initial Git Configuration

* To check your Git version and configuration:  

         $ git --version
         $ git config user.name
         $ git config user.email

* To set user-specific Git configuration (saved in `~/.gitconfig`):  

         $ git config --global user.name "Raymond Rusk"
         $ git config --global user.email "rrusk@shaw.ca"
         $ git config --global color.ui "auto"

        On a Unix, OS X or Cygwin host, configure you favorite
        editor (needed for commit messages):  

         $ export VISUAL = /usr/bin/vim

        or provide a git-specific commit editor configuration  

         $ git config --global core.editor "vim"


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
        
2. Create a local (read-only) Git repo using `git clone`:  

     `$ cd mynewdir`  
     `$ git clone git://github.com/rrusk/git-workshop-notes.git`  
     `$ cd git-workshop-notes`

3. Create a local (read-write) Git repo using `git clone`:  
  (This example only works if your rsa public key is stored on  
   github and I have added you as a collaborator on the project.)
     
     `$ cd mynewdir`  
     `$ git clone git@github.com:rrusk/git-workshop-notes.git # ssh protocol`  
     `$ cd git-workshop-notes`

##Git Usage Overview

* Using Git involves 3-stage thinking - see 3stage.pdf

    * Edit in working directory
    * Add working file to staging (index)
    * Commit staged file to repo  


1. __Adding and committing code__

       `$ vim Notes.md`  
       `$ git status`  
       `$ git add Notes.md`  
       `$ git commit -m"First commit of workshop notes"`

   Alternately, can add and commit in one step

       `$ git commit -am"First commit with no staging"`

   Files can be added to staging by name or by wildcard

       `$ git add submodule1/PrimaryClass.java`  
       `$ git add .`  
       `$ git add *.java`  

   Can interactively add pieces from working file to staging using

       `$ git add -p Notes.md`  

   Note!  Rather than using a sequential revision ID, Git marks
   each commit with an SHA1 hash unique to the person committing the
   changes, the folders, and the files comprising the changeset, allowing
   commits to be independent of a central coordinating server.

   For convenience, the current committed version is also called HEAD.
   To view the SHA1 hash identifying head use

       `$ git rev-parse HEAD`  

   Earlier commits can be referenced by SHA1 or by using

       `HEAD^, HEAD^^, HEAD-1, HEAD-2, etc.`  

2. __Show file changes__

    1. Show unstaged changes (diff working and staging areas)  

        `$ git diff`

    2. Show staged changes (diff staging and repo code)  

        `$ git diff --staged`

    3. Show changes in working and repo code  

        `$ git diff HEAD`  
        `$ git diff HEAD~5 # use 'git rev-parse HEAD~5' to see SHA1`

    4. Limiting diff output  

        1. Show word changes rather than line differences

          `$ git diff --color-words`  
          `$ git diff --word-diff`  

        2. Ignore whitespace  

          `$ git diff -w`

3.  __Show history of commits__

    1. Show all history  

       `$ git log`

    2. Show all history with filenames  

       `$ git log --stat`

    3. Show all history with patches  

       `$ git log -p`  
       `$ git log --patch`

    4. Limit output  

       `$ git log --oneline # one line per commit`  
       `$ git log --max-count=1  
       `$ git log -n        # n = 1,2,3... commits`

    5. Control log format  

       `$ git log --pretty=full  # or fuller, email, raw, format:<pattern>`  
       `$ git log --graph --abbrev-commit --relative --decorate --oneline`

    6. Limit output to added files  

       `$ git log --diff-filter=A`

4. __Ignoring files__

    1. Via local .gitignore  

        `$ vim .gitignore`  

        `#Add patterns to .gitignore, one per line`  
        `*.log`  
        `*.tmp`  
        `target/`  
        `output/`  
        `!wanted.log`  

    2. Objects already in the repository will not not ignored,  
       regardless of their being listed in `.gitignore`.

    3. Gitignore templates for many different development environments  
       are available at [https://github.com/github/gitignore.git](https://github.com/github/gitignore.git)

5. __Removing files__  

     1. Directly remove & stage deletion  

        `$ git rm <FILENAME>`  

     2. Alternately, remove file at OS level and follow up with git add:

        `$ rm <FILENAME>`  
        `$ git add -u .`  


6. __Moving files__

    1. Directly move and stage

        `$ git mv <FILENAME> <NEWFILENAME>`  

     Alternately, at OS level use `mv` and follow up with `git add`:

        `$ mv <FILENAME> <NEWFILENAME>`  
        `$ git add -A .`  

    2. Git tracks file content by similarity index rather than file names.
        To see history of copies and renames use:

        `$ git log --stat -C     # checks modified files as copy source`  
        `$ git log --stat -C -C  # checks unmodified files also, expensive!`  


7. __Aborting work in progress__

     1. For a file that is changed but not staged

        `$ git checkout <file>`  

     2. For a file that is changed and has been staged

        `$ git reset HEAD <file> # removes changes from staging area`  
        `$ git checkout <file>   # removes changes from working file`  

     3. To restore the working copy to the last committed state

        `$ git reset --hard`  

     4. To restore the working copy to a specific commit

        `$ git reset --hard <ref> # <ref> can be tag or SHA-1`  

        1. Beware! When bad commits have already been pushed to a remote repository, resetting to a prior commit can confuse other users sharing the branch.
        2. If you have made bad commits that have been tagged, reset removes them from your branch but not the repository.  They are still accessible through their tags and can be seen with `git log --all`.  To allow the commits to be garbage collected, remove the tags with `git tag -d <tagname>`.

     5. To restore just one file to its previous committed state

        `$ git checkout -- Person.java`  


8. __Check File Authoring using__

        `$ git blame filename.ext`  

    1.  True source of code shown with

        `$ git blame filename.ext -C`  

##More about Cloning URLs
1. File-based url (could be network fileserver)

        $ git clone file://repos/project
        $ git clone /repos/project

2. git url

        $ git clone git://server/project.git

3. ssh url (read-only or read-write)

        $ git clone git+ssh://user@server:project.git
        $ git clone user@server:project.git

4. http url

        $ git clone http://server/project.git
        $ git clone https://server/project.git

##Branches
1. List local branches

        $ git branch

2. List remote branches (but commits are stored locally)

        $ git branch -r

        Remotes are just symbolic names
        The default name is origin if you've cloned

        $ git remote
        $ git remote show origin

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

10. Note on remote branch tracking:    
   `git clone` creates a local repo with all the commits in the  
   remote repository but branches in the remote repository are  
   not treated as local branches.  If you need a local branch  
   that tracks a remote branch use:

   `$ git branch --track <fb> origin/<fb>`

11. Git branches are cheap (20 bytes)

        Create branches for experiments
        Delete failed experiments

## Commit/Push/Pull

1. Commit

        $ git commit
        Saves code snapshot
        Commits to local branch
        Operates on local disk only.

2. Push

        $ git push <remote_repo> <remote_branch>
        Send code to an upstream server
        Update remote branches

3. Pull

        $ git pull <remote>
        Retrieve upstream objects
        Update remote branch (stored locally)
        Merge changes into local branch
        Commit the merge to the local branch

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


## Tagging (cheap)

        Reference, annotated and signed tag types

1. Reference tags
   1. Tag head

        `$ git tag <TAGNAME>`
   2. Tag an existing ref

        `$ git tag <TAGNAME> <REF>`

2. Annotated tags
   1. Tag head with annotated tag

        `$ git tag -a <TAGNAME>`

3. Signed tags
   1. Tag head with signed tag

        `$ git tag -s <TAGNAME>`

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

## Merging

1. Merging a feature branch into master branch

        $ git checkout master
        $ git merge <featurebranch>

2. Merge updates in the master branch to a feature branch you are working on

        $ git checkout <featurebranch>
        $ git merge master

3.  Octopus merge

        $ git checkout master
        $ git merge <fb1> <fb2> <fb3>

4.  Subtree merge (useful if multiple repos are blended together into a single repo)

        $ git checkout master
        $ git merge -s subtree <fb1>

## Rebasing 

Allows merging in code from another branch (often the master branch) while maintaining a clean, linear commit history.

1. Retrieve upstream changes and relocate your
        local changes to the end

        $ git pull --rebase

        Same as
        $ git checkout master
        $ git rebase origin/master

2. Rebase feature branch on master (i.e., add changes on master to feature branch)

        $ git checkout <freaturebranch>
        $ git rebase master

3. Warning: Do not rebase your code once it has already
  been shared with others.  Rebase is most useful for unshared local
  branches that you wish to keep synchronized with another branch.

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

## Revert commits
1. Revert a single commit (will remain visible in branch history)

        $ git revert <ref> # use HEAD as <ref> for most recent commit

2. Revert a range of commits
        (<ref1>, <ref2> must be in right order)

        $ git revert <ref1>..<ref2>

## Amend commits
1. Modify a bad commit message (in HEAD commit)

        $ git commit --amend -m "Much better commit message"

2. Add missing file

        $ git add <missingfile>
        $ git commit --amend


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

## Fetching/Pulling

1. Git fetch commands do not merge (i.e., they retrieve commits from  
   remote repository but do not merge them into local branches)

        $ git fetch
        $ git merge origin/master

2. Git pull commands merge by default

        $ git pull

3. Git pull with rebase

        $ git pull --rebase

   To avoid typing and risky mistakes use

        $ git config branch.autosetuprebase always

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

## Cherry pick merging

   Merge in just one commit (not entire branch)

        $ git cherry-pick a5b2ee
