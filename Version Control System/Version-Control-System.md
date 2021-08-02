# Version Control System

## Table of Contents
- [What is a Version Control System (VCS)?](#what-is-a-version-control-system-vcs)
  - [What is a Distributed Version Control System (DVCS)?](#what-is-a-distributed-version-control-system-dvcs)
- [What is Git?](#what-is-git)
  - [Git Workflow](#git-workflow)
  - [Frequently Used Git Commands](#frequently-used-git-commands)
- [What is GitHub?](#what-is-github)
  - [Setting Up Git and GitHub](#setting-up-git-and-github)
- [Git Branches](#git-branches)
- [Reference](#reference)

## What is a Version Control System (VCS)?
> Version control is a system that records changes to a file or set of files over time so that you can recall specific versions later.
### What is a Distributed Version Control System (DVCS)?
> A system that fully mirrors the repository, including its full history.  
- Every clone is really a full backup of all the data.  
- Can have several remote repositories they can work with, so that we can collaborate with different groups of people in different ways simultaneously within the same project.  

![DVCS](refImg/dvcs.png)

## What is Git?
- Git is a distributed version control system.
- Data is treated like a series of snapshots of a miniature filesystem.
  - *Stream of snapshots.*
- For every commit, Git takes a snapshot of what all the files look like at that moment and stores a reference to that snapshot.  
- If files have not changed, Git doesn’t store the file again. It stores just a link to the previous identical file that it has already stored.  
  ![Git Check Ins Overtime](refImg/git-check-ins-overtime.png)
- Everything in Git is checksummed before it is stored and is then referred to by that checksum.
  - The mechanism that Git uses for this checksumming is called a SHA-1 hash (a 40-character string composed of hexadecimal characters).
- .gitignore includes files that we don't want Git to track and don't want to upload to GitHub.
### Git Workflow
1) You modify files in your working tree.

2) You selectively stage just those changes you want to be part of your next commit, which adds only those changes to the staging area.

3) You do a commit, which takes the files as they are in the staging area and stores that snapshot permanently to your Git directory.
#### The Three States
- The three main states that our files can reside in.  
  ![Three Main States](refImg/three-main-states.png)
##### 1) Working Directory
> The working tree is a single checkout of one version of the project. These files are pulled out of the compressed database in the Git directory and placed on disk for you to use or modify.
- Use **`git add <file(s)>`** to move file(s) to the Staging Area.
- **Untracked**
  - New files that Git doesn't know about or existing files where Git has newly been initialized.
- **Tracked**
  - **Unmodified**
    - Unchanged from the previous version.
  - **Modified**
    - Only files that have changes made to them are moved to the Staging Area.
##### 2) Staging Area
> The staging area is a file, generally contained in your Git directory, that stores information about what will go into your next commit.
- Files that are ready to be saved in the version history.
- Use **`git commit -m "message"`** to save the file(s) to the .git Directory.
##### 3) .git Directory (Repository)
> The Git directory is where Git stores the metadata and object database for your project. This is the most important part of Git, and it is what is copied when you clone a repository from another computer.
- The directory/respository that contains the version history.
- Use **`git checkout fileName`** to return to any previous version.
- Use **`git push origin <branch>`** to push to GitHub.
### Git Configs
- **`git config --list`**
  - Check all configurations for Git.
- **`git config --global core.editor "code --wait"`**
- **`git config --global user.name "Your Name"`**
- **`git config --global user.email "email@email.com`**
- **`git config --global core.autocrlf input`**
  - For mac.
- **`git config --global alias.<abbrv> <commandName>`**
  - Change the specific command name to the designated abbreviation.
  - Ex: `git config --global alias.st status`

### Frequently Used Git Commands
- **`code .`**
  - Opens the current directory in my designated text editor.
- **`git init`**
  - Initialize Git.
  - `ls -al` and `open .git` to see information on the Git.
  - `rm -rf` will remove Git from your project.
- **`git clone <url>`**
  - Clone an existing Git repository.
- **`git status`**
  - Check the Git status of files.
- **`git diff`**
  - Check the changes made between commits, commit and working tree, etc.
- **`git branch`**
  - Check existing branches.
  - Check what branch you're in.
- **`git show-ref`**
  - List the references in the local repository
- **`git add <file(s)>`**
  - Move file(s) to the Staging Area from the Working Directory (after modification).
  - `--all` moves all files including untracked ones.
- **`git commit`**
  - Record changes to the repository.
  - Ex: `git commit -m "[HTML] Edit class names"`
- **`git push origin <branch>`**
  - Updates remote refs using local refs, while sending objects necessary to complete the given refs.
  - i.e., update remote repo with changes made in local repo.
- **`git fetch`**
  - Download objects and refs from another repository.
- **`git checkout <branch>`**
  - Switch branches or restore working tree files.
- **`git checkout <commitId>`**
  - Overwrites your current working directory with the specified snapshot (commit) of your repo from history and make that your new working-set which you can stage and commit as you wish.
  - Example
    ```
    git log // find the commit id you want.
    git checkout <commitId> . // the "." refers to the current directory.
    git commit -m "Restoring old source code" // make a new commit with the restored one.
    ```

## What is GitHub?
- Online service that provides Internet hosting for software development and version control using Git.
### Setting Up Git and GitHub
1. Create a new repository on GitHub
2. Make sure Git is tracking your project.
  - `cd` to your project directory.
  - `git status` to check if git is already initialized.
  - If not, `git init`.
  - Then, go through the process of adding and committing your project to the .git Directory.
3. Connect your local repository to your newly created repository on GitHub.
  - `git remote add origin <copied github repository link>`
    - Copy the repository link ending in ".git" (the web address that your local folder will use to push its contents to the remote folder on GitHub).
  - `git push origin <branch>`
    - Now, push your branch to GitHub.

## Git Branches
> A branch represents an independent line of development. Branches serve as an abstraction for the edit/stage/commit process. You can think of them as a way to request a brand new working directory, staging area, and project history.

> A pointer to a snapshot of your changes. When you want to add a new feature or fix a bug-no matter how big or how small-you spawn a new branch to encapsulate your changes. This makes it harder for unstable code to getm erged into the main code base, and it gives you the change to clean up your future's history before merging it into the main branch.

- *Leave the main branch to always be in deployable state.*

### Core Concepts
- **The "HEAD" Branch**
  - The single currently "active" or "checked out" branch.
- **Local vs Remote Branches**
  - Most of the times we are working with the local branches as we are mostly working on our local machines.

### Working with Branches
#### Creating New Branches
- **`git branch <new-branch-name>`**
  -  Git will start the new branch based on the currently checked out revision (the situation that I was to this point).
  -  **`git branch <new-branch-name> <revision-hash-of-a-particular-commit>`**
    -  Starts the branch on the specified revision.
#### Switching Branches
- **`git switch <other-branch-name>`**
  - Switch to another branch.
- **`git checkout <other-branch-name>`**
  - Switch to another branch.
  - Versatile command that is used for a lot of things.
#### Renaming Branches
- **`git branch -m <new-name>`**
  - Rename the HEAD branch.
- **`git branch -m <old-name> <new-name>`**
  - Renaming a non-HEAD branch.
#### Publishing Branches
- **`git push -u origin <branch-name>`**
  - Upload local branch to the remote repository.
  - `-u` set upstream for git pull/status.
#### Tracking Branches
- Connecting local and remote branches with each other.
  - Ex: you're trying to join in on a remote branch that someone else is working on.
- **`git branch --track <new-local-branch-name> origin/<base-branch>`**
  - Create a local branch with the specified base remote branch.
  - Ex: `git branch --track feature/login origin/feature/login`
- **`git checkout --track origin<base-branch>`**
  - If a local branch name to use is not specified, Git automatically uses the name of the remote branch for the local.
#### Pulling and Pushing Branches
- Synchronizing your local and remote branches.
- **`git pull`**
  - Download new commits from the remote.
- **`git push`**
  - Upload my local commits to the remote.
- **`git branch -v`**
  - Shows any discrepancies in commits between local and remote.
  - Ex: ahead 1, behind 2
#### Deleting Branches
- **`git branch -d <branch-name>`**
  - Delete a branch in your local repository. 
  - *Cannot delete the HEAD branch! So switch to another branch, then delete.*
- **`git push origin --delete <branch-name>`**
  - Delete a branch in the remote repository.
  - *Might also make sense to delete other branches that are tracking that remote branch!*
#### Merging Branches
- Integrating changes (bringing new commits) from another branch *into* your current local HEAD branch.
- **2 Step Process**
  - **`git switch <branch-name>`**
    - Switch to the branch that you want to receive the changes.
  - **`git merge <branch-name>`**
    - Execute the `merge` command with the name of the branch that contains the desired changes.
- Merging creates a merge commit (the "melting point").
#### Rebasing Branches
- An alternative way to integrate changes from another branch *into* your current local HEAD branch.
- **`git rebase <`**
  - There is no separate merge commit that will be created. So, development history appears to have happened in a straight line.
#### Comparing Branches
- Checking which commits are in branch-B, but not in branch-A.
- **`git log <branch-A>..<branch-B>`**
  - Ex: `git log main..feature/uploader`
  - Ex: `git log origin/main..main`

## Reference
[Git - Book](https://git-scm.com/book/en/v2)  
[깃, 깃허브 제대로 배우기 - YouTube](https://www.youtube.com/watch?v=Z9dvM7qgN9s&ab_channel=%EB%93%9C%EB%A6%BC%EC%BD%94%EB%94%A9by%EC%97%98%EB%A6%AC)  
[Git Branches Tutorial](https://www.youtube.com/watch?v=e2IbNHi4uCI&ab_channel=freeCodeCamp.org)  
[The Ultimate GitHub Collaboration Guide | by Jonathan Mines](https://medium.com/@jonathanmines/the-ultimate-github-collaboration-guide-df816e98fb67)
