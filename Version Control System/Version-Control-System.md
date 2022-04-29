# Version Control System

## Table of Contents
- [What is a Version Control System (VCS)?](#what-is-a-version-control-system-vcs)
  - [What is a Distributed Version Control System (DVCS)?](#what-is-a-distributed-version-control-system-dvcs)
- [What is Git?](#what-is-git)
  - [How/Where to Get Git](#howwhere-to-get-git)
  - [Git Workflow](#git-workflow)
  - [Git Configs](#git-configs)
  - [Frequently Used Git Commands](#frequently-used-git-commands)
- [What is GitHub?](#what-is-github)
  - [Setting Up Git and GitHub](#setting-up-git-and-github)
- [Git Branches](#git-branches)
- [Git Clone vs Git Fork and Pull Requests](#git-clone-vs-git-fork-and-pull-requests)
- [Merge Conflicts](#merge-conflicts)
- [Reference](#reference)

## What is a Version Control System (VCS)?
> Version control is a system that records changes to a file or set of files over time so that you can recall specific versions later.
### What is a Distributed Version Control System (DVCS)?
> A system that fully mirrors the repository, including its full history.  
- Every clone is really a full backup of all the data.  
- Can have several remote repositories they can work with, so that we can collaborate with different groups of people in different ways simultaneously within the same project.

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Version%20Control%20System/refImg/dvcs.png" alt="DVCS" width="60%" height="650px" />
</p>


## What is Git?
- Git is a distributed version control system.
- Git helps determine *what* change has been made, *who* made that change, and *why*.
- Data is treated like a series of snapshots of a miniature filesystem.
  - *Stream of snapshots.*
- For every commit, Git takes a snapshot of what all the files look like at that moment and stores a reference to that snapshot.  
- If files have not changed, Git doesn’t store the file again. It stores just a link to the previous identical file that it has already stored.

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Version%20Control%20System/refImg/git-check-ins-overtime.png" alt="Git Checkins Over Time" width="80%" />
</p>
  
- Everything in Git is checksummed before it is stored and is then referred to by that checksum.
  - The mechanism that Git uses for this checksumming is called a SHA-1 hash (a 40-character string composed of hexadecimal characters).
- .gitignore includes files that we don't want Git to track and don't want to upload to GitHub.
### How/Where to Get Git
- Many systems have Git installed by default.
- However, it can be downloaded [here](https://git-scm.com/downloads).
### Git Workflow
1) You modify files in your working tree.

2) You selectively stage just those changes you want to be part of your next commit, which adds only those changes to the staging area.

3) You do a commit, which takes the files as they are in the staging area and stores that snapshot permanently to your Git directory.
#### The Three States
- The three main states that our files can reside in.

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Version%20Control%20System/refImg/three-main-states.png" alt="Three Main States" width="80%" />
</p>

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
  - Initialize Git, effectively creating a hidden .git folder (this is where all of Git's internal tracking data is stored in) inside your repository.
  - `ls -al` and `open .git` to see information on the Git.
  - `rm -rf` will remove Git from your project.
- **`git clone <url>`**
  - Clone an existing Git repository.
  - Downloads a Git repository from the Internet with the latest snapshot of the repository to your working directory on your computer.
  - By default, the contents will be stored in a folder with the same name as the repository from the URL.
    - *The URL is called the **remote origin**.*
- **`git status`**
  - Check the Git status of files.
    - Ex: what files have been modified.
- **`git diff`**
  - Prints a list of the changes that have been made.
  - It is a good way to double-check before committing.
  - `git diff <branch-name> <other-branch-name>`
    - Prints the difference between two branches.
- **`git add <file(s)>`**
  - Move file(s) to the Staging Area from the Working Directory (after modification).
  - `git add --all` moves all files including untracked ones.
- **`git commit`**
  - Record changes to the repository.
  - A commit is a type of checkpoint called a "revision", and is a hash of numbers and letters.
  - Ex: `git commit -m "Fix bug."`
- **`git push origin <branch-name>`**
  - Updates remote refs using local refs, while sending objects necessary to complete the given refs.
  - i.e., update remote repo with changes made in local repo.
  - Alternatively, **`git push origin HEAD`** can be used.
    - `HEAD` refers to the latest commit on your current branch.
- **`git branch`**
  - Check existing branches and check what branch you're in.
- **`git branch <new-branch-name>`**
  - Create a new branch by the name provided.
- **`git checkout <existing-branch-name>`**
  - Switch branches or restore working tree files.
  - `git checkout -b <new-branch-name>`
    - Creates a new branch and checks it out in one go.
- **`git checkout <commitId>`**
  - Overwrites your current working directory with the specified snapshot (commit) of your repo from history and make that your new working-set which you can stage and commit as you wish.
  - Example
    ```
    git log // find the commit id you want.
    git checkout <commitId> . // the "." refers to the current directory.
    git commit -m "Restoring old source code" // make a new commit with the restored one.
    ```
- **`git show-ref`**
  - List the references in the local repository.
- **`git fetch`**
  - Download the latest information about the repository from `origin` (Ex: all the different branches stored on GitHub).
  - Does not change any of your local files; only updates the tracking data stored in the .git folder.
- **`git merge <other-branch-name>`**
  - Takes all the commits existing on the `other-branch-name` branch and integrate them ***into*** your current branch.
  - Since this uses whatever branch data that is stored locally at the moment, run `git fetch` first to download the latest information before merging.
  - Ex: if another developer added some commits to the `master` branch of `origin`, checkout the `master` branch in local, download their changes using `git fetch` and effectively update the `master` branch in local. Then, `git merge origin/master` to merge the origin/master branch into your current branch.
    - `origin/master` means the `origin/master` checkpoint in local. This notation is used to differentiate branches of the same name (Ex: `master`) located in different places (Ex: your own branches vs. origin's branches).
- **`git pull origin master`**
  - Shortcut to both `fetch` and `merge` all in one go.
    - `git pull` does a `git fetch` followed by a `git merge` to update the local repository with the remote repository.
  - Downloads new changes from the branch named `master` on the remote endpoint called `origin` and integrate them into your local HEAD branch.
  - *Make sure to do this (update your repo with the latest version) before creating a new branch to work on.*
  - cf. `git pull origin/master`
    - Pulls changes from the `origin/master` branch on local and merge that to the local HEAD branch.
    - `origin/master` branch is essentially a "cached copy" of the last pull from `origin`.

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
- Process Ex:
  - Push local branch to remote repository.
  - In GitHub, accept the pull request and merge to main branch.
  - In GitHub, delete branch.
  - In local, pull main branch.
  - In local, delete branch.
- **`git pull`**
  - Download new commits from the remote.
  - Ex: `git pull origin main`
- **`git push --set-upstream origin <branch-name>`**
  - If the branch is only on local yet, use this command to upload the branch to GitHub.
  - In GitHub, create the pull request.
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

## Git Clone vs Git Fork and Pull Requests
- The difference comes down to how much control a developer has over a given repository.
### Git Clone
- Anyone can Git clone any repository via the GitHub URL.
  - Ex: `git clone <URL>`
- Changes can be made to this cloned version in local.
- However, only designated contributors are allowed to push back to repository on origin.
  - Undesignated contributors' attempt to push will lead to a 403 error.
  - If you're a collaborator, make sure to clone the repository and not fork it, as you would want to collaborate on the same GitHub repository with your teammates.
#### Flow
- After pushing back to repository on origin, check the branch on GitHub to compare and pull request on that branch.
- Write a brief description of the changes and select the reviewer (the "merge master"), then click "Create pull request".
- Then, proceed to merge the branch to the master branch.
- Once the merge is done, delete the branch.
### Git Fork
- Anyone can fork any repository on GitHub.
  - There is no Git command for forking (use GitHub).
- The repository is completely copied to your own GitHub as a separate entity to the original.
#### Flow
- Clone the fork that has been made to local, and make changes.
- Push back to the repository on origin (the newly forked repository; not the original repository that was forked from).
- It is possible to take the changes made in your forked repository and send them to the original repository. This is called a **Pull Request**.
  - On GitHub, go to your forked repository and click "New pull request" and then "Create pull request".
  - This sends a message to the original repository asking them to "pull" the changes that has been made.
  - The receiver (original repository) will see the pull request and can decide whether or not to accept the changes made and "Merge pull request".
  - Once the merge is done, delete the branch.
### Pull Request
- **The request that your changes be pulled in and merged.**
#### Flow
- Commit your changes to your new branch and then push it to origin.
- Then go to the repository's GitHub page and click on "Pull Request".
  - Pick the branch that you wish to have merged, enter a title and brief description for the Pull Request, and click "Send pull request".
- Once your Pull Request is submitted, a reviewer reviews it and leaves feedback.
- If more changes need to be made, simply add more commits to your existing branch and push it to origin again. The Pull Request will update automatically to reflect your changes.
- By the time your Pull Requested is accepted, there may be a newer version of the trunk (you've been working on an older version of the trunk). Therefore, your changes may not work on the newer version of the trunk.
  - There are two options to get up to date:
    - `git merge master`: applies any new changes from the trunk on top of your work.
    - `git rebase master`: re-applies your work on top of any new changes on the trunk.
    - In both options, your own changes will remain the same as you are essentially just moving your branch up to the top of the trunk to stay up to date with the newest version.
    - It is usually safer to `merge`.

## Merge Conflicts
- **When you made changes to a line of code that someone else has also made changes to, Git is in a conflict as to which version to keep.**
  - The notorious merge conflict output: `>>>>>>> ======= <<<<<<<`
- Essentially, just remove those weird symbols and manually combine the code in between them.
  - Example
    - Merge Conflict Occurred
      ```js
      >>>>>>>
      console.log("Hello, world"); // Branch A
      =======
      console.log("Hello!"); // Branch B
      <<<<<<<
      ```
    - Solved
      ```js
      console.log("Hello, world!");
      ```

## Reference
[Git - Book](https://git-scm.com/book/en/v2)  
[깃, 깃허브 제대로 배우기 - YouTube](https://www.youtube.com/watch?v=Z9dvM7qgN9s&ab_channel=%EB%93%9C%EB%A6%BC%EC%BD%94%EB%94%A9by%EC%97%98%EB%A6%AC)  
[Git Branches Tutorial](https://www.youtube.com/watch?v=e2IbNHi4uCI&ab_channel=freeCodeCamp.org)  
[The Ultimate GitHub Collaboration Guide | by Jonathan Mines](https://medium.com/@jonathanmines/the-ultimate-github-collaboration-guide-df816e98fb67)  
[Understanding Git (part 1) - Explain it Like I’m Five | HackerNoon](https://hackernoon.com/understanding-git-2-81feb12b8b26)  
[Understanding Git (part 2) - Contributing to a Team | HackerNoon](https://hackernoon.com/understanding-git-2-81feb12b8b26)  
[Git Fork vs. Git Clone: What's the Difference? - YouTube](https://www.youtube.com/watch?v=6YQxkxw8nhE&ab_channel=EyeonTech)  
