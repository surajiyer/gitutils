URL(s) to git cheatsheets
---
https://medium.freecodecamp.org/git-cheat-sheet-and-best-practices-c6ce5321f52

Remove a file / directory from the staging area without deleting it locally
---
`git rm --cached <dir_name>`

Delete a file/directory already commited and pushed
---
```
git rm -r --cached <dir_name>
git commit -m 'Remove the now ignored directory.'
git push origin master
```

To delete a file/directory from all commits
---
`git filter-branch --force --index-filter 'git rm --cached --ignore-unmatch <dir-or-file-path>' --prune-empty --tag-name-filter cat -- --all`

To copy files from one branch to another without merging
---
```
git checkout <branch-to-copy>
git checkout <branch-from-copy> <file-path>
git commit -m 'Add file <filename> to <branch-name>.'  # Optional
```

.gitignore everything in current folder
---
`echo [^.]* >> .gitignore`

Delete latest commit. This also deletes the changes.
---
`git reset --hard HEAD~1`

Delete latest commit without delete any of the changes.
---
`git reset --soft HEAD~1`

Delete some commit in the past
---
`git reset --hard <sha1-commit-id>`

If you already pushed it, you will need to do a force push to get rid of it.  
`git push origin HEAD --force`

Check all commits: commits will come with message and a hash
---
`git log`

Edit some historical commit and force push changes
---
```
git checkout <commit hash>
git push -f origin master
```

Delete remote branch
---
`git remote rm origin`

See remote repository urls
---
`git remote -v`

Add remote repository urls as origin
---
```
git remote add origin <url>
git remote set-url origin <url>
```

See list of branches
---
`git branch`

Create branch
---
`git branch <name>`

Checkout branch
---
`git checkout <name>`

Create new branch and checkout to it in one line
---
`git checkout -b <name>`

To merge branch A into B
---
```
git checkout <b>
git merge <a>
```

Pull changes for specific branch
---
`git pull origin <branch>`

To delete a branch locally
---
`git branch -D <branch>`

To delete a branch remotely
---
`git push origin --delete <branch>`

To rename a branch locally
---
```
git checkout <old-branch-name>
git branch -m <new-branch-name>
# *OR*
git branch -m <old-branch-name> <new-branch-name>
```

To rename a branch remotely
---
First delete the old branch and push the new one to remote. Then reset the upstream branch in the new local branch.  
```
git push origin <old-branch-name> <new-branch-name>
git push origin -u <new-branch-name>
```

See which files have changed
---
`git status`

See how much each files have been changed
---
`git diff`

Merge last 4 commits into one commit (will open editor to merge)
---
Discard commit message by changing "pick" to "fixup". Leave "pick" to retain commit message.  
`git rebase -i HEAD~4`

List all files changed in a given commit
---
`git diff-tree --no-commit-id --name-only -r <commit hash>`

Updating local feature branch before merging to master branch
---
Bring your feature branch up to date with master. Deploying from Git branches adds flexibility. Bring your branch up to date with master and deploy it to make sure everything works. If everything looks good the branch can be merged. Otherwise, you can deploy your master branch to return production to its stable state.  
https://gist.github.com/santisbon/a1a60db1fb8eecd1beeacd986ae5d3ca

Perform garbage collection in your repository and prune old objects
---
`git gc --aggressive --prune`

How to compare two files not in repo using git
---
`git diff --no-index file1.txt file2.txt`

Pull all branches in one go
---
https://stackoverflow.com/questions/10312521/how-to-fetch-all-git-branches  
```
git branch -r | grep -v '\->' | while read remote; do git branch --track "${remote#origin/}" "$remote"; done
git fetch --all
git pull --all
```

Undo git commit --amend
---
Move the current head so that it's pointing at the old commit. Leave the index intact for redoing the commit. HEAD@{1} gives you "the commit that HEAD pointed at before it was moved to where it currently points at". Note that this is different from HEAD~1, which gives you "the commit that is the parent node of the commit that HEAD is currently pointing to."  
`git reset --soft HEAD@{1}`

Commit the current tree using the commit details of the previous HEAD commit (Note that HEAD@{1} is pointing somewhere different from the previous command. It's now pointing at the erroneously amended commit.).  
`git commit -C HEAD@{1}`

How to squash all commits in the feature branch (v1)
---
Assuming the feature branch is called feature and the main branch main. Create a temporary branch from main:  
`git checkout -b temp main`

Squash the feature branch in:  
`git merge --squash feature`

Commit the changes (the commit message contains all squashed commit messages):  
`git commit`

Go back to the feature branch and point it to the temp branch:  
```
git checkout feature
git reset --hard temp
```

Delete the temporary branch:  
`git branch -d temp`

How to squash all commits in the feature branch (v2: simpler without rebasing or merging)
---
Checkout the branch for which you would like to squash all the commits into one commit. Let's say it's called feature_branch.
```
git checkout feature_branch
```

Step 1:

Do a soft reset of your origin/feature_branch with your local main branch (depending on your needs, you can reset with origin/main as well). This will reset all the extra commits in your feature_branch, but without changing any of your file changes locally.

```
git reset --soft main  # or master
```

Step 2:

Add all of the changes in your git repo directory, to the new commit that is going to be created. And commit the same with a message.

```
git add ...
git commit -m "commit message goes here"
git push -f  # if previous commits were already pushed else without --force
```

Change default text editor for git
---
`git config --global core.editor "nano"`

Checkout the latest tag
---
```
git fetch --all --tags
tag=$(git describe --tags `git rev-list --tags --max-count=1`)
git checkout $tag -b latest
```
