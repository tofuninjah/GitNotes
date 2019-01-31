# Git Notes | Viewing History

<hr>

* **git status**
* **git log**
* **git show**
* **git diff**

## Git status
    
###### Untracked State
* Not under version control
* Git does not track any changes to these file(s) over time

###### Tracked & Modified
* Changes under version control
* Any changes from the previous commit will be tracked

###### Changes to be committed
* Considered _Staged_
* Final change in the lifecycle

### Lifecycle
**untracked -> [git add] -> staged -> [git commit] -> unmodified**

**modified -> [git add] -> staged -> [git commit] -> unmodified**

## Git log

###### 5 most recent commits

    git log -5

###### Condensed log

    git --oneline

###### Log for just one file

    git log --oneline [file1-example.txt]


###### Show graph

    git log --graph

###### Graph with Condensed log 

    git log --graph --decorate --oneline

## Git show

    git show

* Similar to git log
* **Good to use for seeing information about a single commit**
* Shows only the most recent commit, with the changes

##
        git show --name-status

* List of files that changed, and how they changed

##

    git show HEAD~2:file-2.txt

* Output contents of file-2.txt two commits ago

## Git diff

    git diff

* Only shows changed to modified files

<hr>

    git diff --staged

* Shows changes on stage

<hr>

    git diff file-2.txt

* Show only changes in this file

<hr>

    git diff -w

* Ignore noise while using diff
* This will ignore the whitespace
* No changes will show if the changes were only whitespace

<hr>

    git diff <sha#>

* This will output all the changes from the <sha#> to the current commit.

<hr>

    git diff <sha#1>..<sha#2>

* Two previous commits and ignore the current changes.
* **Older commit 1st and the more recent commint 2nd**

