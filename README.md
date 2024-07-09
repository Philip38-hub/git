# GIT
This project is designed to introduce one to the world of version control and collaboration using Git. It explores the specifics of git and how it is useful in a production environment where developers work on a project remotely.
# Lessons learned
- Initializing git repositories: bare or unbare
- Commiting and pushing my work to a remote repository
- Working with branches within a version control
- Branching, merging, fetching and pulling changes
# Git commits to commit
## Initialize the git repository
- While in the work directory, run ``` git init ```. This initializes a branch with a default name master. This can be changed by using ``` git config --global init.defaultBranch <name> ```

## Check the status and act accordingly with the output of the executed command.
```
    git status
```
- Add hello.sh to be tracked
```
    git add hello.sh
```

## Stage the changed file and commit the changes, the working tree should be clean.
```
    git add hello.sh
```
```
    git commit -m "chore: modified hello.sh to a bash script"
```
- Check if working tree is clean by ``` git status ```
## Add a comment and stage chages
```
    git add hello.sh
```
## Two seperate commit
### for comment on line 3
```
    git commit -m "chore: add comment at line 3"
```
### for changes on line 4 and 5
```
    git commit --allow-empty -m "chore: changes at line 4 and 5"
```

# History
## Show history of a working directory
```
git log
```
## Show One-Line History for a condensed view showing only commit hashes and messages.
```
git log --oneline
```
## Controlled Entries
```
git log -n 2 --since="5 minutes ago"
```
## Personalized Format
```
git log --pretty=format:"* %h %ad | %s (%d) [%an]" --date=short

```
# Check it out
## Restore First Snapshot
- Get the hash of the initial commit
```git log --oneline```
- Checkout initial commit obtained above
```
git checkout 816895c
```
- Display contents of hello.sh
```
cat hello.sh
```
## Restore Second Recent Snapshot
- Get the hash of the second recent commit
```
git log --oneline
```
- checkout the second most recent commit obtained above
```
git checkout 57fb092
```
- Display contents of hello.sh
```
cat hello.sh
```
## Return to Latest Version
```
git checkout master
```

# TAG me
## Referencing Current Version
```
git tag v1
```
## Tagging Previous Version without relying on hashs for navigation
```
git tag v1-beta HEAD^
```
## Navigating Tagged Versions
- From v1 to v1-beta
```
git checkout v1-beta
```
- From v1-beta to v1
```
git checkout v1
```
## Listing Tags
```
git tag
```

# Changed your mind?
## Reverting Changes
```
git restore hello.sh
```
## Staging and Cleaning
- stage changes
```
git add hello.sh
```
- Unstage or clean the staging area to discard changes
```
git restore --staged hello.sh
```
## Committing and Reverting
- add and commit unwanted changes on hello.sh
```
git add hello.sh
```
```
git commit -m "chore: commit unwanted changes to be reverted"
```
- Revert unwanted changes on hello.sh that are committed already
```
git revert 2944b1c
```
## Tagging and Removing Commits
- add a tag oops
```
git tag oops
```
- Remove commits after v1 and ensure head is pointing at v1
```
git reset --hard v1
```
## Displaying Logs with Deleted Commits
```
git reflog
```
## Cleaning Unreferenced Commits
```
git gc --prune=now
```
## Author Information
- add author information ``` git add hello.sh ``` and commit ``` git commit -m "chore: add author details" ```
- update the file to include the email without making a new commit/ add change to the last commit
```
git add hello.sh
```
```
git commit --amend --no-edit
```
# Move it
## Move hello.sh to lib/ and commit move
```
git mv hello.sh lib/
```
```
git commit -m "Move hello.sh into the lib/ directory"
```
## Create make file in root directory of repository
```
touch Makefile
```
- While at the root directory hello of the repository ``` git add makefile ``` then commit makefile ``` git commit -m "add Makefile for project build automation" ```
# blobs, trees and commits
## Exploring .git/ Directory
- branches/:

    This directory is used by older versions of Git for storing branch information. It's generally empty in modern Git versions because branches are now managed within the refs/ directory.

- COMMIT_EDITMSG:

    This file contains the commit message of the most recent commit. When you run git commit, Git opens this file in your text editor for you to edit the commit message.

- config:

    This file stores configuration settings for the repository. These settings can include user information, remotes, and other repository-specific settings.

- description:

    This file is used by GitWeb to provide a description of the repository. It's generally not used by standard Git operations.

- HEAD:

    This file points to the current branch reference. It typically contains a reference like ref: refs/heads/main, indicating the current active branch.

- hooks/:

    This directory contains client- or server-side scripts that are triggered by Git events, such as pre-commit, post-commit, pre-push, etc. These hooks can be used to enforce policies or automate workflows.

- index:

    This file is also known as the staging area. It contains information about what will go into the next commit, including file changes that have been added but not yet committed.

- info/:

    This directory contains auxiliary information. For example, the exclude file in this directory can be used to specify patterns for files to be ignored in this specific repository.

- logs/:

    This directory contains logs of changes to the tips of branches. Each file in logs/refs/ keeps a record of all commits made to the respective branch. This can be useful for recovering lost commits.

- objects/:

    This directory stores all the content (files and directories) in the repository in an object database. Each object is identified by its SHA-1 hash. The objects can be of four types: blobs (file data), trees (directories), commits (commit data), and tags (tag data).

- ORIG_HEAD:

    This file is used to store the original HEAD reference before a potentially dangerous operation like a rebase or merge. It's used to help you recover if something goes wrong.

- packed-refs:

    This file stores references in a packed format for efficiency. It's used to speed up operations on repositories with a large number of references (branches or tags).

- refs/:

    This directory contains references to commit objects. It's subdivided into heads/ for local branches, remotes/ for remote-tracking branches, and tags/ for tags.
## Latest Object Hash
- Get the latest object hash in .git/objects/ directory
``` 
git rev-list --objects --all | git cat-file --batch-check='%(objectname)' | sort -k 2 -n | tail -n 1 | awk '{print $1}'
```
- Print type and content of this object
```
git cat-file -t f786e01c456b7133e5a8a8947a03b3d15cab636f
```
```
git cat-file -p f786e01c456b7133e5a8a8947a03b3d15cab636f
```
## Dumping Directory Tree
- dump directory tree in commit above
```
git ls-tree -r f786e01c456b7133e5a8a8947a03b3d15cab636f
```
- Dump contents of the lib/ directory and the hello.sh file using Git commands
```
git ls-tree HEAD:lib/
```
```
git show HEAD:lib/hello.sh
```

# Branching
## Create and Switch to New Branch
``` git checkout -b greet ``` or ``` git checkout -c greet ``` (in newer versions of git)
- Create greeter.sh and commit
``` touch greeter.sh ```
``` git add greeter.sh ``` , ``` git commit -m "chore: add greeter.sh" ```
## Update the lib/hello.sh
``` git add hello.sh ``` then ``` git commit -m "chore: modify hello.sh ```
## Update the Makefile
``` git add Makefile ```  then ``` git commit -m "chore: update Makefile" ```
## Switch back to the main branch, compare and show the differences between the main and greet branches for Makefile, hello.sh, and greeter.sh files.
- back to main branch
``` git checkout master ```
- show differences
``` git diff master greet -- Makefile hello.sh greeter.sh ```
## add README.md
``` git add README.md ``` then ```  git commit -m "doc: add README.md" ```
## Draw a commit tree diagram illustrating the diverging changes between all branches to demonstrate the branch history.
```
git log --graph --oneline --all
```
# Conflicts, merging and rebasing
## Merge Main into Greet Branch
- Switch to greet ``` git checkout greet ```
- merge ``` git merge master ```
- switch back to master ``` git checkout master ```, make changes to hello.sh and commit ``` git add hello.sh ``` then ``` git commit -m "modify hello.sh at branch master" ```
## Merging Main into Greet Branch (Conflict)
- attempt to merge master to greet, resolve conflicts and complete merge
``` git merge master ```
- after resolving conflict
``` git add hello.sh ``` then ``` git commit ```
## Rebasing greet Branch
- get commit hash ``` git log --oneline ```
- rebase ``` git reset --hard 3f7a111 ```
- Rebase the greet branch on top of the latest changes in the main branch
``` git rebase master ```
- if conflicts exist, resolve and run:
``` git rebase --continue ```
## Merging Greet into Main
- switch to master and then,
``` git merge greet ```
## Understanding Fast-Forwarding and Differences
- fast forwarding - is a type of merge where the HEAD of the current branch is simply moved forward to point to the latest commit of the branch being merged. This can only happen when there are no divergent commits on the current branch, meaning the current branch is a direct ancestor of the branch being merged
- Merging combines the changes from two branches. It creates a new commit that includes changes from both branches. This is particularly useful when the branches have diverged.
- rebasing - moves or combines a sequence of commits to a new base commit. It rewrites the commit history, placing the commits from one branch onto another branch as if they were created from that branch.

# Local and remote repositories
## Clone the repository hello as cloned_hello. (Do not use copy command)
- While in the work directory
```
 git clone hello cloned_hello
```
## Show the logs for the cloned repository.
- Navigate to the cloned repo ``` cd cloned_hello ``` then ``` git log ```
## Display the name of the remote repository and provide more information about it.
- Display name ``` git remote ```
- Provide name plus more information ``` git remote show origin ```
## List all remote and local branches in the cloned_hello repository.
- All local branches ``` git branch ```
- All remote branches ``` git branch -r ```
- All branches ``` git branch -a ```
## Make changes to the original repository, update the README.md file with the provided content, and commit the changes.
- While in hello directory ``` git add README.md ``` then ``` git commit -m "doc: add readme in the hello depository for instruction's requirements" ```
## Inside cloned_hello, fetch the changes from the remote repository and display the logs. Ensure that commits from the hello repository are included in the logs.
- Fetch from remote repo ``` git fetch origin ``` and display all logs ``` git log --oneline --graph --all ```
## Merge the changes from the remote main branch into the local main branch.
- After fetching from remote repository, ``` git merge origin ``` to merge changes
## Add a local branch named greet tracking the remote origin/greet branch.
```
git checkout --track origin/greet
```
## Add a remote to your Git repository and push the main and greet branches to the remote.
- add a remote called main ``` git remote add main https://learn.zone01kisumu.ke/git/pochieng/another_work_repo.git ```
- push main ``` git push -u main master ``` then push greet ``` git push -u main greet ```

# Bare repositories
## What is a bare repository and why is it needed?
- Bare repository is a repository that does not have a working directory. Unlike a typical Git repository (also known as a non-bare or standard repository), which includes a working directory where files are checked out and edited, a bare repository contains only the Git revision history and its related metadata, without any checked-out version of your project files
- It is needed for central repository where people collaborate to work on a project. All the tracked changes are stored and prevent any conflict between the versions of the project on other’s computers
## Create a bare repository named hello.git from the existing hello repository.
```
git clone --bare . ../hello.git
```
## Add the bare hello.git repository as a remote to the original repository hello
```
 git remote add bare ../hello.git
```
## make changes to README.md Commit the changes and push them to the shared repository.
- make changes add ``` git add README.md ```, commit ``` git commit -m "doc: update README.md for pushing to shared repo hello.git" ``` then push to hello.git ``` git push bare master ```
## Switch to the cloned repository cloned_hello and pull down the changes just pushed to the shared repository.
- While in cloned_hello, add the hello.git as bare if not available. If it is available, run
```
git pull bare master
```