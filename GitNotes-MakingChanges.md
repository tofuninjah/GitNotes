# Git Notes | Making Changes
<hr>

## Adding files to be tracked
    
**Interactive Mode: Patch**

    git add -p

* Gathers all Changes in _Tracked_ files and reviews the changes!
* Stage this hunk [y,n,q,a,d,e,?]?
    * y - stage this hunk
    * n - do not stage this hunk
    * q - quit; do not stage this hunk or any of the remaining ones
    * a - stage this hunk and all later hunks in the file
    * d - do not stage this hunk or any of the later hunks in the file
    * e - manually edit the current hunk
    * ? - print help

**Once staged using the interactive mode:**

        git commit -m 'Message here!'

<hr>
    git add -A

* Stage All (new, modified, deleted) files
* Will also track all untracked files 

<hr> 
    git add .

* Stage All (new, modified, deleted) files

<hr>
    git add --ignore-removal .

* Stage New and Modified files only

<hr> 
    git add -u

* Stage Modified and Deleted files only
* Skips untracked files, and only adds tracked files


## Committing Files

    git commit -m 'Adding additional files'

* Comments added inline on Terminal prompt 

<hr>
    git commit -a -m 'add + commit in one step'

* Add and Commit in one step
* Only performs this for _Tracked_ files 

<hr>

    git commit --amend

* Amends the previous commit message
* Useful to add additional changes to previous commit

<hr>

    git commit -a --amend --no-edit

* This will add all Tracked changes to the _previous_ commit!

<hr>

## Removing Files from a Git Repository

    git rm file2-example.txt

* Removes file from repository

<hr>

    git rm --cached file-3.txt

* Removes the file from version control, but keeps the file on local as _untracked_
* Results in git seeing file-3.txt as a new untracked file.

## Resetting

    git reset new-file.txt

* Undos the git add command
* If you add a file to be tracked, but want to untrack it.
* Changes to the file will still be there.

<hr>

    git reset

* Resets _ALL_ stage changes
* Undo commands like `git add` and `git rm`
* This will undo the stage changes, but the modifications to the file will still be there in local as _Modified_.

<hr>

    git reset sha70d7213^ 

* Needs the carrot and the end (sha followed by the carrot)
* Using git reset to reset git commits or all of current changes to the previous commit

* Undo the commit and place the changes back into the index.  Resets the git repository to a state right before a commit

* deleted, tracked, and untracked will be still on local:

        $ git status
        On branch develop
        Your branch is up to date with 'origin/develop'.
        
        Changes not staged for commit:
          (use "git add/rm <file>..." to update what will be committed)
          (use "git checkout -- <file>..." to discard changes in working directory)
        
                deleted:    CHANGELOG.md
                modified:   README.md
                modified:   index.js
        
        Untracked files:
          (use "git add <file>..." to include in what will be committed)
        
                new.html
        
        no changes added to commit (use "git add" and/or "git commit -a")
        
<hr>

    git reset --hard

* Undo all changes -> hard to undo -> reconfirm your intentions

## git revert
    
**Automatically create new commit containing changes that undo the original commit!**

    git revert <sha#>

* Lets say you already have a README.md in your project that is tracked.  Someone, or you make a change to it and commit it.  Then you can do `git revert sha#`, sha# being this commit sha number.  Git will automatically create a new commit undoing the original one!!!

<hr>

**Undo some changes, or undo commit as well as add additional changes - you can use the -m option:**

    git revert -n <sha#>

* Take the commit <sha#>, and use `git revert -n <sha#>`
* Now we are in the state of REVERTING: 
        
        ckang@TXHQCKANG-L1 MINGW64 ~/Work/GitRepos/vue-clip (develop|REVERTING)
* Now we have the option to do `git add` or `git rm` to stage changes we want to undo as well as add more changes!

**When done:**

    git revert --continue

* Use commit message to highlight the changes to show this revert was _not_ done automatically

**Aborting:**

    git revert --abort
