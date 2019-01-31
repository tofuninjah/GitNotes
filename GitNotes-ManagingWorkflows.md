# Git Notes | Managing Workflows

<hr>

* **git branch**
* **git checkout** 
* **git merge**
* **git rebase**
* **git cherry-pick**

## Git branch

    git branch

* Highlights the branch we are currently on
* Also shows the branches on your local machine

<hr>

    git branch new-branch

* Create a new branch

<hr>

    git branch -d remove-me

* Create a new branch

<hr>

    git checkout branch-name

* Checkout a branch / Switch between branches

<hr>

    git checkout -b another-branch

* Shorthand to checkout and create a branch

<hr>

## Switching Branches

**Before switching branches, the current branch must be in a _clean_ state.**


    git checkout -- file-2.txt

* This will undo modified changes to a file, and make the branch _clean_

<hr>

    git checkout <sha#>(previous commit) file1-txt

* This will checkout a file from a previous commit

<hr>

    git checkout <sha#> 

* To checkout a previous version of the repository at this commit
* You will receive a warning of being in a detached HEAD state. --> **All this mean is that you are not on a branch** You can make new commits but git wont track them unless you make a new branch, so this is good to browse a previous version of a repository.
* This is good to look around, but if you want to look around and make changes it's better to create a branch FROM the previous version.     

<hr>

    git branch initial-branch <sha#>(Previous Commit)
    
                          OR
    
    git checkout -b initial-branch <sha#>(Previous Commit)

*  This will create a new branch from the previous commit sha#. 
*  This way you can start from this commit, and make additional commits and changes and they will all be tracked.

<hr>

## Git merge

**Merging changes from one branch into an new branch**

    git checkout master
    git merge new-branch

* Pass git merge the new-branch while you are _**ON**_ the branch you want to merge _**into**_.

## Merge Conflict and Merge Commits

**Create a change on a new-branch, then make another change to the same file on master branch**


* Once you try to merge:

        $ git merge new-branch
        Auto-merging file1.txt
        CONFLICT (content): Merge conflict in file1.txt
        Automatic merge failed; fix conflicts and then commit the result.

<hr>

    git log --oneline --decorate --graph --all
    
    ckang@TXHQCKANG-L1 MINGW64 ~/Work/GitRepos/getgit (master|MERGING)
    
    $ git log --oneline --decorate --graph --all
    * 8660cd4 (HEAD -> master) testing
    * 0d73b66 conflict resolve
    | * a033cea (new-branch) teest
    |/
    * efb7901 initial
    
    $ cat file1.txt
    <<<<<<< HEAD
    Wed, Oct 17, 2018 12:55:52 PM
    Wed, Oct 17, 2018 12:56:03 PM
    Wed, Oct 17, 2018 12:59:18 PM
    =======
    Wed, Oct 17, 2018 12:58:41 PM
    >>>>>>> new-branch

* Once merge conflict is resolved, you can commit it.

        ckang@TXHQCKANG-L1 MINGW64 ~/Work/GitRepos/getgit (master)
        $ git log --oneline --decorate --graph --all
        *   ce1506e (HEAD -> master) resolve conflict
        |\
        | * a033cea (new-branch) teest
        * | 8660cd4 testing
        * | 0d73b66 conflict resolve
        |/
        * efb7901 initial

## Git rebase
* Sometimes the branch you are working on falls behind the one you branched from. Normally this happens because changes were committed and merged _after_ you created your branch.
* Git rebase will bring your branch back up to date with another branch and re-apply any commits you've made _on top_.
* Effectively, git rebase will make it as if you just created your branch and did your work.

<hr>

    git rebase master


* Rewinds branch, then replays all of the commits then applies your branch on top of it!
* Conflicts can happen the same as merging.  You would resolve them the same way.

<hr>

    git rebase -i <sha#>(last commit I want to keep)^
    EX: git rebase -i ed9f042^

* Interactive Mode - Allows modifying a set of commits
* **the sha#^ is the last commit you want to _keep_. The carrot is needed to have it be included in the rebase**
* You can also use git rebase to interactively change our commit history
* **Can be useful for cleaning up a branch before merging.**

        /# Rebase a033cea..ce1506e onto a033cea (2 commands)
        /#
        /# Commands:
        /# p, pick = use commit
        /# r, reword = use commit, but edit the commit message
        /# e, edit = use commit, but stop for amending
        /# s, squash = use commit, but meld into previous commit
        /# f, fixup = like "squash", but discard this commit's log message
        /# x, exec = run command (the rest of the line) using shell
        /# d, drop = remove commit
        /#
        /# These lines can be re-ordered; they are executed from top to bottom.
        /#
        /# If you remove a line here THAT COMMIT WILL BE LOST.
        /#
        /# However, if you remove everything, the rebase will be aborted.
        /#
        /# Note that empty commits are commented out

<hr> 

* p: keep commit
* r: edit the commit message
* e: pause the rebase process, so you can make more commits before continuing
* s: condense the commit into the previous, and also change the commit message
* f: condense the commit without editing the commit message
* x: run command using the shell.
* d: remove the commit, or you can just delete the line

<hr>
###### Another example of Git Rebase use

**Say you have 3 commits and you need to amend the wording on the 3rd previous commit**

	CKang@TXHQCKANG-L2 MINGW64 ~/Work/Code/Repositories/labs-java-junit (master)
	$ git logo
	
	a05c13c (HEAD -> master) Repairs to LunEx libraries
	2654c5d Fixing TestedTrek encoding AGAIN
	2deacad Strip returns Ammend this!!!!!
	27e442d The Original Files
	4946f85 Initial commit 



**Lets amend the commit 2deacad -> to be "Strip returns this is changed"**



	git rebase -i HEAD~3
	
	...
	
	pick 2deacad Strip returns Ammend this!!!!!
	pick 2654c5d Fixing TestedTrek encoding AGAIN
	pick a05c13c Repairs to LunEx libraries
	
	# Rebase 27e442d..a05c13c onto 27e442d (3 commands)
	#
	# Commands:
	# p, pick <commit> = use commit
	# r, reword <commit> = use commit, but edit the commit message
	# e, edit <commit> = use commit, but stop for amending
	# s, squash <commit> = use commit, but meld into previous commit
	# f, fixup <commit> = like "squash", but discard this commit's log message
	# x, exec <command> = run command (the rest of the line) using shell
	# d, drop <commit> = remove commit
	# l, label <label> = label current HEAD with a name
	# t, reset <label> = reset HEAD to a label
	# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
	# .       create a merge commit using the original merge commit's
	# .       message (or the oneline, if no original merge commit was
	# .       specified). Use -c <commit> to reword the commit message.
	#
	# These lines can be re-ordered; they are executed from top to bottom.
	#
	# If you remove a line here THAT COMMIT WILL BE LOST.
	#
	# However, if you remove everything, the rebase will be aborted.
	#
	# Note that empty commits are commented out

**This is similar to patch, where now you will use reword.  Note: You cannot change the commit message here.  Once you close it, git will bring another screen to actually edit the commit message**

    reword 2deacad Strip returns Ammend this!!!!!
    pick 2654c5d Fixing TestedTrek encoding AGAIN
    pick a05c13c Repairs to LunEx libraries

**The next Screen Prompt you can edit the commit message**

#### Note!!

	git rebase -i HEAD~3

is the same as:

	git rebase -i <commit sha#>^

**The first example starts with the HEAD (1st) and gets the 2nd and 3rd previous commit (2deacad).  The 2nd example takes the specific commit and uses the ^ to include it in the selection.**

<hr>

## Cherry-pick

**Times when you have another branch and you don't want to merge _ALL_ the commits from this other branch, and it's more work to rebase them**

    git cherry-pick <sha#>

* Do that command on the branch you want to cherry-pick into.  Ex: You are in master branch and you have another branch that you want to pick a commit to merge it into the master branch.
* Once you cherry pick, be aware that the <sha#> has changed on your branch that you currently on (the branch you are cherry-picking into)
* Git sees both commits as different even though they have the same changes.
* **Only use cherry pick when you don't intend to merge the _WHOLE_ branch!**

        git cherry-pic <sha#1> <sha#2>
                    
                       OR
        
        git cherry-pick <sha#range>^..<sha#range>


* Cherry-pick 1st commit, 2nd commit.  OR get a range!

    git cherry-pick -n <sha#>

* **-n** Applied the changes, but does **NOT** apply the commit, you would then manually commit the change.

## Squash

	CKang@TXHQCKANG-L2 MINGW64 ~/Work/Code/Repositories/spring-webflux-reactive-rest-api-demo (master)
	$ git logo
	* 2ddba48 (HEAD -> master) SQUASHED!
	* 5f37b68 Initial Commit
	
	git rebase -i 2ddba48^

In VIM:
	
	# Changed this from pick 2ddba48 SQUASHED!
	squash 2ddba48 SQUASHED!
	
	# Rebase 5f37b68..2ddba48 onto 5f37b68 (1 command)
	#
	# Commands:
	# p, pick <commit> = use commit
	# r, reword <commit> = use commit, but edit the commit message
	# e, edit <commit> = use commit, but stop for amending
	# s, squash <commit> = use commit, but meld into previous commit
	# f, fixup <commit> = like "squash", but discard this commit's log message
	# x, exec <command> = run command (the rest of the line) using shell
	# d, drop <commit> = remove commit
	# l, label <label> = label current HEAD with a name
	# t, reset <label> = reset HEAD to a label
	# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
	# .       create a merge commit using the original merge commit's
	# .       message (or the oneline, if no original merge commit was
	# .       specified). Use -c <commit> to reword the commit message.
	#
	# These lines can be re-ordered; they are executed from top to bottom.
	#
	# If you remove a line here THAT COMMIT WILL BE LOST.
	#
	# However, if you remove everything, the rebase will be aborted.
	#
	# Note that empty commits are commented out

#### Note!

**The Squash above will leave you with two commits.  The initial Commit and the squashed. One option to squash a initial commit is with the command below: **

	git rebase -i --root
