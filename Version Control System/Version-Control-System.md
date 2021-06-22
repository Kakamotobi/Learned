# Version Control System

## Table of Contents
- [What is a Version Control System (VCS)?](#what-is-a-version-control-system-vcs)
  - [What is a Distributed Version Control System (DVCS)?](#what-is-a-distributed-version-control-system-dvcs)
- [What is Git?](#what-is-git)
  - [Git Workflow](#git-workflow)
  - [Frequently Used Git Commands](#frequently-used-git-commands)
- [What is GitHub?](#what-is-github)
  - [Setting Up Git and GitHub](#setting-up-git-and-github)
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



## Reference
[Git - Book](https://git-scm.com/book/en/v2)  
[깃, 깃허브 제대로 배우기 - YouTube](https://www.youtube.com/watch?v=Z9dvM7qgN9s&ab_channel=%EB%93%9C%EB%A6%BC%EC%BD%94%EB%94%A9by%EC%97%98%EB%A6%AC)
