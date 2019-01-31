# Git Notes | Sharing Work

<hr>


## Git clone

    git clone git@github.com:tofuninjah/a-repository.git

* Clones to your local in a-repository folder.

<hr>

    git clone git clone git@github.com:tofuninjah/a-repository.git a-folder

* Clones in a-folder directory

<hr>

    git clong a-folder a-folder2

* Git clone can copy a local repository without downloading!  

## Git remote

** **origin** is automatically created that references the clone URL.**

    $ git remote -v
    origin  git@github.com:tofuninjah/gettinggit.git (fetch)
    origin  git@github.com:tofuninjah/gettinggit.git (push)
 
* _fetch_ and _push_ are by default the same, but can be changed if they are located in different directories/servers.

<hr>

    git remote add upstream git@github.com:gettinggit/gettinggit.git

* Create additional remote for repository, that way when updates happen, we can pull them down to our fork.

<hr>

    $ git remote -v
    origin  git@github.com:tofuninjah/gettinggit.git (fetch)
    origin  git@github.com:tofuninjah/gettinggit.git (push)
    upstream        git@github.com:gettinggit/gettinggit.git (fetch)
    upstream        git@github.com:gettinggit/gettinggit.git (push)

* Upstream is a common name for the parent repository.  It will contain the work that will flow into our repository.

<hr>

    git remote rename upstream parent

* Rename remote upstream named 'Upstream' by default to 'Parent'. 

<hr>

    $ git remote -v       
    origin  git@github.com:tofuninjah/gettinggit.git (fetch)
    origin  git@github.com:tofuninjah/gettinggit.git (push)
    parent  git@github.com:gettinggit/gettinggit.git (fetch)
    parent  git@github.com:gettinggit/gettinggit.git (push)

* Now the upstream remote is called _parent_

<hr>

    git remote set-url origin https://a-url.com/a-repository.git

* This will change the URL of the remote.

<hr>

    git remote remove parent

* This will remove a repository
* Multiple remotes are common for large teams or contributing

## Git push

**Sharing Work === Sharing Commits**

> After making some changes...

    git push origin register

> Pushes to our fork

    $ git push origin register
    Counting objects: 3, done.
    Delta compression using up to 4 threads.
    Compressing objects: 100% (2/2), done.
    Writing objects: 100% (3/3), 276 bytes | 69.00 KiB/s, done.
    Total 3 (delta 1), reused 0 (delta 0)
    remote: Resolving deltas: 100% (1/1), completed with 1 local object.
    To github.com:tofuninjah/gettinggit.git
       9175652..b5e30cc  register -> register

<hr>

    git push -u origin HEAD

> the **-u** flag sets the tracking branch for future use!

    git push origin HEAD

> This is all you need to use to push now. Instead of `git push origin [branch-name]`

* By using HEAD we can keep git push consistent across multiple branches
* As long as the **current** branch you are on matches the one in the **remote**, we can use this and it will work.
* Now we don't have to specify a remote name, it will use the one from our tracking branch or origin by default.

<hr>

    git push origin HEAD:a-brand-spankin-new-branch

* Push current branch to remote branch with a different name!

<hr>

    git push origin :a-brand-spankin-new-branch

* This will delete a remote branch!!!
* In effect, it will push _NOTHING_ to a remote branch which removes it.

> Results in:
    
    $ git push origin :a-brand-spankin-new-branch
    To github.com:tofuninjah/gettinggit.git
     - [deleted]         a-brand-spankin-new-branch

## Force push

** There are times when your local branch gets behind a remote branch**

> Example: someone pushes on the same branch that you were working on.

    $ git log --oneline --decorate --all
    b5e30cc (origin/register, origin/force-push, register) Signing the registry
    9175652 (HEAD -> force-push, origin/master, origin/HEAD, master) Use "course" instead of "series"
    c265319 Merge pull request #6 from jasonmccreary/signing-in
    518c81f Signing the registry
    c1de4dd Adds detail on purpose of repository
    ...

> This shows that the local branch (register) is behind the remote branch (master).

    git push origin HEAD

> Since our branch is behind by one commit it will result in a being 'rejected!'

    $ git push origin HEAD
    To github.com:tofuninjah/gettinggit.git
     ! [rejected]        HEAD -> force-push (non-fast-forward)
    error: failed to push some refs to 'git@github.com:tofuninjah/gettinggit.git'
    hint: Updates were rejected because the tip of your current branch is behind
    hint: its remote counterpart. Integrate the remote changes (e.g.
    hint: 'git pull ...') before pushing again.
    hint: See the 'Note about fast-forwards' in 'git push --help' for details.

> If we still want to push our changes, we can use the -f or force option

    git push -f origin HEAD

> Outputs: 

    $ git push -f origin HEAD
    Total 0 (delta 0), reused 0 (delta 0)
    To github.com:tofuninjah/gettinggit.git
     + b5e30cc...9175652 HEAD -> force-push (forced update)

* Overwrites the commit history (meaning we lost the commit to the register branch)

**Force push === overwrite remote history with local history**

**Be Careful!! Examine intentions and notify team**

## Git fetch

**Eventually your local copy will get behind the remote**

* does not update your local copy

> git fetch = download latest references from remote


    fetch === downloads

    fetch !== updates

**Useful for gathering new information from a remote and determine if your local is out-of-sync!**

## Git Pull

**If we find our local copy is out-of-date by using `git fetch` or by being told, we can use `git pull`**

    $ git log --oneline --decorate --all
    b5e30cc (origin/register, register) Signing the registry
    9175652 (origin/master, origin/force-push, origin/HEAD, force-push) Use "course" instead of "series"
    c265319 Merge pull request #6 from jasonmccreary/signing-in
    518c81f Signing the registry
    c1de4dd Adds detail on purpose of repository
    cbedd4f (HEAD -> master) Add link to video transcript
    a93e6e2 Add example to registry
    ...

> We can see our local master (HEAD -> master) is behind origin master branch

    git pull origin master

> This will bring our branch up-to-date

    $ git log --oneline --decorate --all
    b5e30cc (origin/register, register) Signing the registry
    9175652 (HEAD -> master, origin/master, origin/force-push, origin/HEAD, force-push) Use "course" instead of "series"
    c265319 Merge pull request #6 from jasonmccreary/signing-in
    518c81f Signing the registry
    c1de4dd Adds detail on purpose of repository
    ...

> local master is back inline with origin master!!!!

* git pull = git fetch + git merge
* Might encounter merge conflicts when running `git pull`

<hr>

**When we git pull and a commit message comes up**

> You can prevent the merge by clearing the message (empty commit message)
> Then you can run

    git merge --abort

> To reset the repository to the state it was in before we ran git pull

    git pull --rebase origin master

> Bring our branch up-to-date then apply our changes

    ckang@TXHQCKANG-L1 MINGW64 ~/Work/GitRepos/gettinggit (master)
    $ git status
    On branch master
    Your branch and 'origin/master' have diverged,
    and have 1 and 4 different commits each, respectively.
      (use "git pull" to merge the remote branch into yours)
    
    nothing to commit, working tree clean
    
    ckang@TXHQCKANG-L1 MINGW64 ~/Work/GitRepos/gettinggit (master)
    $ git pull --rebase origin master
    From github.com:tofuninjah/gettinggit
     * branch            master     -> FETCH_HEAD
    First, rewinding head to replay your work on top of it...
    Applying: Adding a new file

**Helpful in a pinch, but shouldn't be your workflow**


