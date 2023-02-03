# Tips 

[https://firstaidgit.io/](https://firstaidgit.io/)

## Merge my local changes with another branch
### modify undo commit after push reset

This can be done while a regular `merge`, and you should *keep your merge history* by making sure to use `--no-ff`, which means _no fast forward_. 

 Switch to the branch you're merging against, make sure it's up to date, and merge: 

 `git merge <the-other-branch> --no-ff` 

 You should get a commit message with the `merge X into Y branch`, anthen you can safely push your merge.>


## Undo commit before pushing changes
### modify undo commit local before push reset

If you made a commit that you wish to modify or erase entirely, the git `reset` command can be of help here. 

 `git reset HEAD~1 # undo last commit, keep the changes` 

 `git reset --hard HEAD~1 #undo last commit, erase the changes` 

 Naturally, be careful when using the last option as your local files will be discarded! 

 To keep the samcommit message:

 `git commit -a -c ORIG_HEAD`

 You will be prompted with your last commit message. 

 Tips from [hrbonz](https://github.com/hrbonz) 


## Recover a deleted branch
### recover deleted branch"

By typing `git reflog`, you can grab the commit hash (SHA1) at the top of your deleted branch. Copy this `sha`, then use: 

 `git checkout <sha>` 

 Once you've done that, recover the branch by typing: 

 `git checkout -b <branchname>`

git reflog


## Display the commits that have deleted files
### display show deleted files history

You can quickly check which commits included deleted files by using the following command: 

 `git log --diff-filter=D --summary` 

 This will show you the commits in which files were removed.


## Rebase my branch with master
### rebase branch against master changes

To catch-up with the latest changes from master (or any other branch you started from), you can and should rebase against it. Say you're working on the `foobar` branch: 

 `git checkout foobar` 

 Followed by a rebase: 

 `git rebase master` 

 This applies the `origin` commits on top of master. When you're done with solving conflicts, continuby using `git rebase --continue`. At this point you can continue to work on your branch or merge it against master. 

 Learn more about [rebases](https://github.com/k88hudson/git-flight-rules#i-need-to-combine-commits)


## Restore a deleted file
### restore recover deleted file

If you deleted a file by accident, you can get it back by checking it out again: 

 `git checkout myFile.txt` 

 If you need to restore a file from a certain point in time in your commit history, grab the hash of that commit and run: 

 `git checkout $commit~1 myFile.txt`


## Discard local file changes
### revert changes reset file specific version

The easiest way to get rid of unwanted changes is by resetting a file or a folder to the current commit state. To do so, you can use 

 `git checkout myFile.txt` 

 You can also reset a specific path instead: 

 `git checkout -- myPath`


## Clear all stashed states
### clear stash

You can clear all stashed states by using: 

 `git stash clear`


## Remove untracked files and folders
### remove clear untracked files"

To remove all untracked files from your working copy: 

 `git clean -f` 

 To remove all untracked files and folders: 

 `git clean -fd` 

 Tip: If you'd like to see first which files will be untracked before actually removing them, you can do a safe clean test by running: 

 `git clean -n`

git clean


## Undo a commit message before pushing
### undo commit message before pushing amend

You can amend your commit message by typing `git commit --amend`, which will open your editor and allow you to make changes to the most recent commit message. 

 You can also modify the message directly, by using: 

 `git commit --amend -m \"My new awesome message\"`


## Display commits by author
### display show commits by author name"

You can filter the commit history by author by using: 

 `git log --author=\"AuthorName\"`

git log author


## Search for a specific commit message in all commits
### search commit message regexp log find

You can search for a specific commit log by matching it on a regular expression. Use: 

 `git log --grep <your-query>`


## Undo a commit message after pushing
### undo commit message after pushing amend

This is a two-step process. You need to amend your commit message by using `git commit --amend`, and then you re-write your branch commit history by force pushing the commit: `git push <remote> <branch> --force` 

 **Warning:** by force pushing, you can lose the remote branch commits if your local branch is not up to date, so be careful.


## Combine two or more commits
### combine commits join rebase"

You will need to use an interactive rebase. If you're rebasing against master, start the process by typing `git rebase -i master`. However, if you're not rebasing against a branch, you'll need to rebase against your `HEAD`.

 If you want to squash your last 2 commits, you can use: 

 `git rebase -i HEAD~2`.

 You will then be prompted to folloinstructions to pick commits. If you wish to combine all your commits with the oldest *first* commit, leave the first line with `pick` and change the letter to `f` on all other commits. Learn more about [rebases](https://github.com/k88hudson/git-flight-rules#i-need-to-combine-commits) 

git rebase


## Remove a file from git but keep the local file
### remove file from git delete

This will remove the file your your git tracking, but keep your local copy:

 `git rm --cached myfile.txt`


## Compare an old revision of a file
### compare old revision file"

You can easily view the contents of a file at a specific point in time by using: 

 `git show commitHash:myFile.txt`

git show


## Squash feature branch commits to merge into the release branch
### squash combine commits feature merge update release"

If you decide to merge and squash your commits, this will create a new commit but only in the release branch, therefore the history of the feature branch will remain intact. 

 Here's an example on how to achieve this: 

 `git fetch origin` 

 `git checkout [release-branch]` 

 `git rebase origin/[release-branch]` 

 `git merge —squas—no-commit [feature-branch]` 

 `git commit -m 'Merge X into Y'` 

 You'll end up with one commit only in your release branch, while keeping the feature history intact. 

 [Learn more about feature branches](https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow)

git squash


## Reset to a certain commit in history
### revert reset commit certain history

If you don't care about your local changes, you can reset back to one commit back in time by doing a hard reset: 

 `git reset --hard HEAD~1` 

 This will set the HEAD back by one commit. You can also do this using the commit hash instead.


## Untrack files that already exist
### untrack remove files already exist there

If you want to untrack a file that already exists in the repo but would still like to keep it locally, commit your changes and run: 

 `git rm -r --cached` 

 This will remove changed files from the staging area. Afterwards, just run a normal: 

 `git add .` and commit your changes.


## Recover a deleted tag
### recover undo deleted tag accident

If you want to recover a tag deleted by accident, start by grabbing it: 

 `git fsck --unreachable | grep tag` 

 Once you found it, you can restore it: 

 `git update-ref refs/tags/NAME KEY` 

 [Source](http://java.dzone.com/articles/git-tip-restore-deleted-tag)


## Move uncommited changes to a new branch
### create new branch keep changes

Too often we start changing files on a branch to fix something, only then realising we didn't create a new branch before. Luckily, this is as simple as creating a new branch: 

 `git checkout -b my-new-branch-name` 

 This will bring any files from your current branch into the new one, which you can commit then.


## Push locally created branch to remote
### push local branch remote

If you've created a local branch but now want it to be tracked on the remote you can run: 

 `git push -u origin my-new-branch-name` 

 Now everyone can checkout to your branch.


## Recover stashed changes
### recover stash stash@ changes stashed"

If you still have your changes stashed, you can apply them onto your branch by using `git stash apply`. You can run `git diff` to compare the differences. To get rid of the stash afterwards run: 

 `git stash drop` 

 If you have more than one stash, find the one you need by running: 

 `git stash list` and applying it by referencing its index

 `git stash@{1}` 

 Keep in mind that this is zero-based (`stash@{0}` is the first one) 

 [Read more about stashes](http://stackoverflow.com/q/19003009)

git checkout stash


## Rename my local and remote branch
### rename wrong name branch

Let’s say you have a _“fix-bug25”_ branch, and you want to rename it to _“hotfix-users”_. First, change the local branch: 

 `git branch -m fix-bug25 hotfix-users` 

 Now, the remote branch: you can’t rename the branch directly, so you’ll need to delete it and push it again with the new name. Make sure no one else within your team is working on itDelete the branch: `git push origin --delete fix-bug25` 

 And now let's push the new one: `git push origin hotfix-users`


## Undo a commit after pushing it to master
### undo revert commit pushed after push"

This will revert one or more commits that you want to erase from the remote branch. You can either specify one by hash: 

 `git revert b712c3c` 

 Revert only the second to last commit: 

 `git revert HEAD^` 

 Or undo the last commit without creating a revert commit: 

 `git revert -n HEAD` 

 Tip: `HEAD~1` is a shortcut to `HEAD^`, themean the same thing. [Learn more about git revert](https://www.atlassian.com/git/tutorials/undoing-changes/git-revert)

git revert


## Unstage a file
### unstage file from remove commit

To unstage a file added by mistake, the easiest option would be: 

 `git reset HEAD unlovedFile.txt`

### Committed to the wrong branc### commit wrong branch master accident

Start by switching to the new branch you forgot to create, by typing 

 `git checkout -b <new-branch>`. 

 Switch back to the original branch now: 

 `git checkout <original-branch>` 

 ...and reset to the last commit you want to keep. 

 To do so, you can type `git log` and save the hash (SHA1) of the last commit that you want to keep. Let'say this is `a31a45c`. 

 Now you have to reset it: `git reset --hard a31a45c` and push this last forced reset. 

 **Warning:** Make sure no one has committed to that original branch in the meantime, or changes might be lost!


## Delete remote branch
### delete remote branch

If you are positive this branch can be deleted, type: 

 `git push origin --delete branch-name`


ssuming that you have no changes that you want to save, this can be done with two easy commands. 

 First let's get a fresh fetch of the remote: 

 `git fetch <remoteBranchName>`. 

 Then let's tell git that we want to reset our local to the head of our remote branch like so: 

 `git reset --hard origin/<localBranchName>`. 

 If you do hav### you wish to keep, you should check out a new branch and make a commit before resetting: `git commit -m \"just in case\"` 

 `git branch my-new-branch-name`"
### Reset local branch to match remote


reset local branch to match remote


## Show number of commits from each contributor
### show number commits

Want to see how many commits everyone in your team has made? 

 `git shortlog -s -n` 

 This will order the authors in decending order by number of commits made.


## Show all the commits to one file
### show all commits file"

Ever wanted to see all the commits made to a single file? 

 `git log --follow -p -- myfile` 

 This will show all the commits made to a single file. The `--follow` argument ensures that we can see commits made to that file even if it was under a different file name at the time. 

 Omit the `-p` to just see the commit messages and not the commicontent.

git log follow


## Use command alias's in your CLI
### command alias

Getting fed up of typing `git status`? Well we can easily alias that command to something slightly quicker to type within git. 

 `git config --global alias.st status` 

 Now all we have to type is `git st` 

 We can take it a step further by aliasing more complicated commands to an alias: 

 `git config --global alias.logme 'log ---author=Rob'` 

 Now we've aliased `git logme` to show all of our commits.


## Mark a conflict file as resolved
### conflict mark resolve file"

To mark a file (or several) conflicted files as resolved so you can push the changes up, normally add these files: 

 `git add <file>` 

 You can then type `git commit` to solve these conflicts and push the changes up. 

 Suggested by Robert Wünsch.

git add

## Display all unpushed commits
### display show unpushed commits"

To view all the commits that are yet to be pushed to all branches, use the following command: 

 `git log --branches --not --remotes` 

 Alternatively, you can also use: 

 `git log origin/master..HEAD`

git log

## Rename a git tag
### rename tag name change"

To rename an existing tag: 

 `git tag <newTag> <oldTag>` 

 `git tag -d <oldTag>` 

 `git push origin :refs/tags/<oldTag>` 

 `git push --tags`

git tag

## Remove old remote git branches
### remove old prune branch"

If a branch is removed on the remote, you can delete it from the local refs by using the `git-remote prune <name>` feature. 

 This will delete the stale remote-tracking branch under <name>, where the stale branch has already been removed from the remote repository referenced by <name>, but are still locally available in `remotes/<name>`.

git remote prune <remote>


## Update a specific submodule
### update submodule"

To update a specific submodule in your repository, you should append the path to your submodule: 

 `git submodule update --remote --merge <path>` 

 Suggested by Wouter Peschier.

git submodule remote merge


## Stage deleted files
### stage commit deleted files folders"

To stage for commit files or folders that you have deleted locally, you can use: 

 `git add -u` 

 If you only want to stage the current path you're in, use: 

 `git add -u .`

git add -u


## Move recent commits to a new branch
### move recent commits to new branch last"

Say you want to move the last 2 commits into a new, separate branch. You can do this by creating the branch, rolling back 2 commits and checking out the branch. So: 

 `git branch <branch-name>` 

 `git reset --hard HEAD~2 # this rolls back 2 commits` 

 `git checkout <branch-name>` 

 Note that by HARD resetting you will lose any uncommitework. So make sure that everything is committed!

git reset branch


## Show all remote branches
### show branches remote display"

To view a list of all tracked and untracked remote branches, use: 

 `git remote show origin`

git add -u


## List all files changed in a commit
### show branches remote display"

To view **just** the file names, use: 

 `git show --name-only SHA1`

Also view the status:

 `git show --name-status SHA1`

git show


## Checkout a file from a different branch
### checkout file different branch"

You can checkout a single file from a different branch without switching to it. Use: 

 `git checkout <branch_name> -- <file_path>`

git checkout


## Show contents of a file at a specific commit
### Show contents of a file at a particular commit"

You can get a file’s contents out from a specific commit. Use: 

 `git show <treeish>:<file>` 



 [Source](http://gitready.com/intermediate/2009/02/27/get-a-file-from-a-specific-revision.html)

git show


## Clone a repository without getting the entire history
### git clone"

You're looking for a shallow clone. This will only fetch the latest history and not all the objects in the repository, which might take a long time while not necessary. 

 `git clone <repository URL> --depth 1` 

 The `depth` parameter allows you to specify how deep you want to go: 

 `git clone <repository URL> --depth 5`

clone shallow depth


## Add a remote repository
### remote add origin"

Before you can push to a remote, you first need to add one. When you clone a repository, git will automatically add the URL you cloned from as the `origin` remote. To add one yourself, use: 

 `git remote add <remote eame> <repository URL>` 

 For example: 

 `git remote add upstream https://github.com/magalhini/firstaidgit.git`

git remote add


## List all remote branches
### remote branch list all"

To list all remotes: 

 `git remote` 

 This will only show the names of the remotes. To view more information, use `git remote show`.

git remote


## Show information about a remote branch
### remote branch show information"

To show information about a remote branch, such as fetch and push URLs, tracked branches and HEAD, use: 

 `git remote show origin` 

 Where `origin` is your remote.

git remote show


## Delete specified tag
### delete tag for local and remote"

You can delete a specific tag locally using: 

 `git tag -d [tag name]` 

 Remove tag from remote using: 

 `git push [remote name] :refs/tags/[tag name]` 



git tag
  
  ## Rename a remote
### rename a remote"

To rename a remote: 

 `git remote rename <oldRemote> <newRemote>` 

 You can verify your changes by listing the remotes: 

 `git remote` 

 [Source](https://help.github.com/articles/renaming-a-remote/)

git remote rename <oldRemote> <newRemote>
  
   {
## Remove a submodule
### remove submodule rm delete"

To remove a submodule use: 

 ``git rm <submodule_path>``

 In case you later want to add a submodule with the same name, you will also need to use:

``rm -rf .git/modules/<submodule_path>``

git rm

## Compare commit log difference between 2 branches
### commit log difference between 2 branches"

You can compare difference in commit log between 2 branches by: 

 `git log [branch one]..[branch two]` 

 Example to compare the **staging** and **development** branch logs: 

 `git log staging..development` 

git log

## List untracked files within untracked directories
### list untracked files within untracked directories"

To list untracked files within untracked directories: 

 `git status -u` 

By default, git will not show files within directories that are untracked.

git status