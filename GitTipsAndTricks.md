# Introduction #
This article assumes you know how to work with GIT in a local-repo, single-branch mode. It's not an introduction to GIT.
I've found this [video](http://excess.org/article/2008/07/ogre-git-tutorial/) useful for understanding GIT.

# Sending a patch #
## Repository layout ##
Working in a branch (`git checkout -b branch_name`) will make all of the following steps easier.

## Working locally ##
Commit intermediate results locally (`git commit -a` or `git add file ; git commit`) - these will be squashed together before pushing to the remote repository.

## Sending a patch for review ##
We use http://codereview.appspot.com for code reviews. Download [upload.py](https://codereview.appspot.com/static/upload.py) for uploading patches.
Then, assuming you've worked in a branch, you can tell `upload.py` what to use as diffbase - the parameter after `--` specifies that:
```
upload.py -- master
```
Will create a diff from master.
If you're not working off a branch, then you could use `upload.py -- <hash of latest master commit>` to create the diff.

## Addressing code review comments ##
By working off a branch the process is identical to sending a patch for review.
To catch up with changes on the master, do:
```
git checkout master
git pull
git checkout -b branch_name
git rebase master
```
Rebasing from master will replay local changes on top of the latest changes from the remote repository. That will present with the opportunity to solve any merge conflicts. Should a merge conflict occur, fix it manually then use `git add` to mark the conflicts as resolved and `git rebase --continue` to continue the rebase process.

# Submitting a patch #
Each commit should be meaningful by itself, so unless you want to explicitly expose your local work steps, squashing intermediate commits together to a single one before pushing is recommended.

## Squashing commits ##
If you've worked off a branch, this is a straightforward affair.
start by running:
```
git checkout master
git log |head
```

and noting the hash of the latest commit. Then:
```
git merge branch_name
git rebase -i <hash of commit from previous step>
```

Git will then open up $EDITOR with a list of commits to squash.
**P\*ick a single commit and mark the rest as**S**so they are squashed together with the one you've picked. The commit message of the**p\*icked commit will be the one used.


## Actually pushing ##
Switch to master (`git checkout master`), verify your commit is on top of the latest one (using `git log`) and is properly squashed, then:
```
git push
```