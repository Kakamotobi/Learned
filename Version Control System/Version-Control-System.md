# Version Control System

## Table of Contents
- [File System](#file-system)
- [What is a Version Control System (VCS)?](#what-is-a-version-control-system-vcs)
  - [What is a Distributed Version Control System (DVCS)?](#what-is-a-distributed-version-control-system-dvcs)
- [What is GitHub?](#what-is-github)
- [What is Git?](#what-is-git)
  - [Git Installation](#git-installation)
  - [The Three States of a File](#the-three-states-of-a-file)
  - [The Three Main Sections of a Git Repository](#the-three-main-sections-of-a-git-repository)
    - [The Relationship between the Three Sections](#the-relationship-between-the-three-sections)
- [Git Objects](#git-objects)
- [Git Configurations](#git-configurations)
- [Git Stash](#git-stash)
- [Git Tag](#git-tag)
- [Undoing Changes/Commits and Time Travelling](#undoing-changescommits-and-time-travelling)
  - [Moving Back and Forth in Time (Detached Head)](#moving-back-and-forth-in-time-detached-head)
  - [Discard Current Changes / Unmodify Files](#discard-current-changes--unmodify-files)
  - [Undoing Commits](#undoing-commits)
    - [Applying Undone Commits on Local to Remote](#applying-undone-commits-on-local-to-remote)
  - [Amending Previous Commit Messages](#amending-previous-commit-messages)
- [Git Branches](#git-branches)
  - [Core Concepts](#core-concepts)
    - [The "HEAD" Branch](#the-head-branch)
    - [Relative Refs](#relative-refs)
    - [Local Branches vs Remote Branches](#local-branches-vs-remote-branches)
  - [Branch Commands](#branch-commands)
    - [Creating a New Branch](#creating-a-new-branch)
    - [Switching Branches](#switching-branches)
    - [Renaming Branches](#renaming-branches)
    - [Tracking Branches](#tracking-branches)
    - [Pushing and Pulling Branches](#pushing-and-pulling-branches)
    - [Deleting Branches](#deleting-branches)
    - [Combining Branches](#combining-branches)
      - [Merging Branches](#merging-branches)
      - [Rebasing Branches](#rebasing-branches)
        - [When Not to Rebase](#when-not-to-rebase)
    - [Comparing Branches](#comparing-branches)
- [Git Remote](#git-remote)
  - [Remote Tracking Branch](#remote-tracking-branch)
  - [Upstream](#upstream)
- [Git Clone and Git Fork](#git-clone-and-git-fork)
  - [Git Clone](#git-clone)
  - [Git Fork](#git-fork)
- [Pull Request](#pull-request)
  - [How it Works](#how-it-works)
  - [Workflow - only local and origin](#workflow---only-local-and-origin)
  - [Important Notes](#important-notes)
- [Merge Conflicts](#merge-conflicts)
- [Git Collaboration Workflows](#git-collaboration-workflows)
  - [Centralized Workflow](#centralized-workflow)
  - [Feature Branch Workflow](#feature-branch-workflow)
  - [Fork & Clone Workflow](#fork--clone-workflow)
- [Frequently Used Git Commands](#frequently-used-git-commands)
- [Reference](#reference)

## File System
> In computing, a **file system** or **filesystem** (often abbreviated to **fs**) is a method and data structure that the operating system uses to control how data is stored and retrieved. | Wikipedia

- A filesystem is needed to avoid storing everything as one large body of data, which means we cannot _determine where one data piece of data starts and stops_.
- Through the file system, every data is separated and given a name, which makes it possible to identify that data/file.
- File Systems are used in HDs, SSDs, etc.
- RAM uses a temporary file system (since it is volatile memory).

## What is a Version Control System (VCS)?
> Version control is a system that records changes to a file or set of files over time so that you can recall specific versions later.
### What is a Distributed Version Control System (DVCS)?
> ...**distributed version control** (also known as **distributed revision control**) is a form of version control in which the complete codebase, including its full history, is mirrored on every developer's computer. | Wikipedia

- Every clone is really a full backup of all the data.  
- Can have several remote repositories they can work with, so that we can collaborate with different groups of people in different ways simultaneously within the same project.

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Version%20Control%20System/refImg/dvcs.png" alt="DVCS" width="60%" height="630px" />
</p>

## What is GitHub?
- An online service that provides Internet hosting for software development and version control using Git.
- It is a platform for managing remote repositories.
  - `git remote add <remote-name> <github-repo-link>` sets the web address (alised as `remote-name`) of the remote repository on GitHub that your local repository will keep track of.

## What is Git?
- **Git is a distributed version control system.**
  - Git takes a snapshot of the whole directory rather than of only the changes.
    - For every commit, git takes a snapshot of what all the files look like at that moment and stores a reference to that snapshot.
    - If files have not changed, Git doesn’t store the file again. It stores just a link to the previous identical file that it has already stored.
  - Data is treated like a series/stream of snapshots of a miniature filesystem.
- **Git stores each file and directory based on the _hash_ of its contents.**
  - Git is able to quickly determine if any changes have been made by comparing hash values (not the entire file).
- Git helps determine *what* change has been made, *who* made that change, and *why*.  
- Everything in Git is checksummed before it is stored and is then referred to by that checksum.
  - The mechanism that Git uses for this checksumming is called a SHA-1 hash (a 40-character string composed of hexadecimal characters).

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Version%20Control%20System/refImg/git-check-ins-overtime.png" alt="Git Checkins Over Time" width="80%" />
</p>

### Git Installation
- Many systems have Git installed by default.
- However, it can be downloaded [here](https://git-scm.com/downloads).
### The Three States of a File
- The three main states that our files can reside in are: **modified**, **staged**, and **committed**.
- **Modified**
  - Changes have been made to the file but it has not been commited yet.
- **Staged**
  - The modified file has been marked as ready to be included in your next commit snapshot.
- **Committed**
  - The staged file has been included in the snapshot in your local database.
### The Three Main Sections of a Git Repository
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Version%20Control%20System/refImg/three-main-states.png" alt="Three Main States" width="80%" />
</p>

#### 1) Working Directory
> The working tree is a single checkout of one version of the project. These files are pulled out of the compressed database in the Git directory and placed on disk for you to use or modify. | Git

- **Untracked**
  - New files that Git doesn't know about or existing files where Git has newly been initialized.
- **Tracked**
  - **Unmodified**
    - Unchanged from the previous version.
  - **Modified**
    - Only files that have changes made to them can be moved to the Staging Area.
- It contains the files that you are currently working on, including any changes that have been made to them.
  - i.e. the files and folders that you can see on your computer.
- Use **`git add <file(s)>`** to move the modified file(s) to the Staging Area.
  - cf `git update-index --add <file(s)>` is a lower-level command that adds files to the index (staging area) even if they don't exist in the working directory.
#### 2) Staging Area
> The staging area is a file, generally contained in your Git directory, that stores information about what will go into your next commit. | Git

- **a.k.a. the index.**
- (Staged) Files that are ready to be snapshot.
- Use **`git commit`** to save the file(s) to the .git Directory.
  - Making a commit has two phases.
    - `git write-tree`
      - Take the current index state and create an object describing that entire index.
      - i.e. all files and their contents are tied together and hashed to create a git directory object (essentially, the snapshot).
    - `git commit-tree`
      - Commit a tree into a commit object.
#### 3) .git Directory (Repository)
> The Git directory is where Git stores the metadata and object database for your project. This is the most important part of Git, and it is what is copied when you clone a repository from another computer. | Git

- **The directory/respository that contains the version history.**
- _The .git Directory contains all necessary things for a git repository including the HEAD, the index file, objects, refs, config, etc._
  - The **HEAD file** that contains a specific reference (Ex: `refs/head/main`).
  - The **index file** describes the current working tree.
  - The **".git/objects/"** path contains the files that have been hashed.
  - The **".git/refs/"** path contains references to objects.
- _This is the "local" repository that contains all the snapshots._
  - The files and folders on your computer do not represent the local repository.
  - The HEAD branch refers to the currently checked out branch (in the .git directory).
    - i.e. switching branches shows different files/folders, which is possible through this ".git" directory.
#### The Relationship between the Three Sections
- **When a new change is made.**
  - (Working Directory !== Staging Area) && (Working Directory !== HEAD)
  - Staging Area === HEAD (at .git Repository)
- **When a new change is staged.**
  - Working Directory === Staging Area
  - (Working Directory !== HEAD) && (Staging Area !== HEAD)
- **When a new commit is made.**
  - Working Directory === Staging Area === HEAD
#### Git and Remote Flow
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Version%20Control%20System/refImg/git-remote-flow.png" alt="Git Remote Flow" width="80%" />
</p>

## Git Objects
- Git is based on a key-value data store.
  - i.e. any content can be inserted into a git respository. This content will be hashed as a unique key that can be used later to retrieve the content.
- Note
  - An object is not the file itself. It is merely the contents of the file.
  - Even if the file is changed later, that object remains the same, as objects are _immutable_.
- `git hash-object <filename>`
  - Creates a unique object ID with the filename as the "key", and the actual file as the value.
- `ls .git/objects/??/*`
  - Check objects in the object database.
- `git cat-file -t <object name(hash)>`
  - Get the type of the specified object.
- `git cat-file blob <object name(hash)>`
  - See the contents of the specified object.
- A **commit object** contains:
  - a commit message.
  - the author.
  - the parent commit.
  - a tree object (containing its ancestors).
    - `blob` types that represent files.

<p align="center">
  <img src="https://git-scm.com/book/en/v2/images/data-model-1.png" alt="Git tree and blob example" width="60%" />
</p>

## Git Configurations
- **`git config --list`**
  - Check all configurations for Git.
- **`git config --global core.editor "code --wait"`**
  - Set VSCode as default editor.
- **`git config --global user.name "<Your Name>"`**
- **`git config --global user.email "<Your Email>"`**
- **`git config --global core.autocrlf input`**
  - For mac.
- **`git config --global alias.<Abbrv> <Command Name>`**
  - Change the specific command name to the designated abbreviation.
  - Ex: `git config --global alias.st status`

## Git Stash
- Stash uncommitted changes so that we can return to them later, without having to make unncesseary commits.
- Stashes are kept in a stack of stashes.
- **`git stash`**
  - Takes all uncommitted changes (staged and unstaged) and stash them, reverting the changes in your working copy.
  - We don't see the stashed changes but they are still available.
- **`git stash pop`**
  - Removes the most recently stashed changes in your stash and reapply them to your working copy.
- Use Cases
  - When you do not want to make a commit yet.
  - When you do not want the changes from another branch to come with you to another branch.
- Example
  - Master is unchanged and you created and switched to a new branch.
  - You made changes to the new branch (no commits yet).
  - Then you try to switch to the master branch (to check something, need to quickly edit a minor typo, etc.).
    - If there are no conflicts between the two branches,
      - The changes that were made in the new branch will come along and show up in the master branch.
      - If you had committed the changes in the new branch, and then switched back to the master branch, the changes will not come along.
    - If there are conflicts between the two branches,
      - You will get an error saying "Your local changes to the following files would be overwritten by checkout".
      - Therefore, you need to commit your changes or stash them before you switch branches.

## Git Tag
- Mark certain commits as "milestones" that can be referenced like a branch.
- _Tags never move despite further commits._
- Checking out a tag results to detached HEAD since you cannot commit directly on the tag.
- **`git tag <tag name> [<commit>]`**
  - By default, tags the HEAD.
  - `git describe <ref>`
    - `<ref>` is anything that git can resolve into a commit.
    - Outputs `<tag>_<num commits>_<hash>`.

## Undoing Changes/Commits and Time Travelling
### Moving Back and Forth in Time (Detached Head)
- **`git checkout <id-of-previous-commit>`**
  - HEAD moves to that particular commit (detatched HEAD).
- **`git checkout HEAD~1`**
  - HEAD moves back by 1 commit.
- **`git switch <branch-name>`**
  - Comes out of detatched HEAD state.
- **`git switch -`**
  - HEAD moves to whatever branch it was last.
### Discard Current Changes / Unmodify Files
- **`git checkout HEAD <file-name>`** or **`git checkout -- <file-name>`**
  - Reverts the file back to it's last committed version.
- **`git restore <file-name>`**
  - Restore the file to the contents in the HEAD.
  - _This command is not "undoable". Uncommited changes will be lost._
  - `git restore --source <source-name> <file-name>`
    - Restore the file to that particular commit.
    - The default source is HEAD.
      - The source can be a commit hash or HEAD~.
    - Ex: `git restore --source HEAD~1 app.js`.
  - This does not time travel or detatch the HEAD. You are still on the last commit (tip of the master branch). But the file has been modified.
    - `git restore <file-name>` to go back to the HEAD (the most recent commit).
  - `git restore --staged <file-name>`
    - Unstage files.
### Undoing Commits
- **`git reset <commit-hash | file>`**
  - Reset the repository back to a specific commit, but with the the changes still in the working directory.
  - All commits that proceed the specific commit are removed.
    - The branch pointer is moved backwards, eliminating the commits as if they never occured in the first place.
  - `git reset --hard <commit-hash>`
    - Resets the repository (HEAD) back to a specific commit and removes the actual changes in the files.
    - Any changes (in tracked files) that were made after the specific commit is destroyed and removed from the local directory.
    - Any untracked files are simply deleted.
  - `git reset --soft <commit-hash>`
    - Resets the repository (HEAD) back to a specific commit and keeps the changes in the files.
    - Changes that were made after the specific commit are kept as "changes to be committed".
  - ***If you want to reverse commits that you have not shared with others, use reset.***
- **`git revert <commit-hash>`**
  - Creates a brand new commit which reverses/undos the changes from the specified commit.
  - The proceeding commits of the specified commit are not removed (keeps them in record) but the changes are gone.
  - Reverting can cause conflicts.
  - ***If you want to reverse some commits that other people already have on their machines, use revert.***
#### Applying Undone Commits on Local to Remote
- **`git push origin -f`**
  - Force push the current branch on local to the corresponding branch on origin.
- **`git push origin +HEAD`**
  - Force a non-fastword(`+`) push so that the top commit on origin is the parent of the current top commit.
### Amending Previous Commit Messages
#### Amending the Most Recent Commit Message
- **`git commit --amend`**
  - Modify the most recent commit.
  - It allows for us to combine currently staged changes with the previous commit, rather than creating a new commit.
  - It opens your text editor, in which you can edit and save the commit message. Then, force push the branch to remote.
#### Amending Older or Multiple Commit Messages
- **`git rebase -i HEAD~n`**
  - Displays a list of the `n` last commits on the current branch.
  - It opens your text editor, in which you can edit and save the corresponding commit messages. Then, force push the branch to remote.

## Git Branches
> A branch represents an independent line of development. Branches serve as an abstraction for the edit/stage/commit process. You can think of them as a way to request a brand new working directory, staging area, and project history. | Atlassian

> A pointer to a snapshot of your changes. When you want to add a new feature or fix a bug-no matter how big or how small-you spawn a new branch to encapsulate your changes. This makes it harder for unstable code to get merged into the main code base, and it gives you the change to clean up your future's history before merging it into the main branch. | Atlassian

- **A branch is essentially a pointer/reference to a specific commit.**
  - Generating a new branch is simply making a new variable that references the specified commit.
  - Therefore, there is no store/memory overhead in making branches.
  - _Switching branches is merely moving HEAD to another pointer, and changes the local to the specified pointer's contents._
### Core Concepts
#### The "HEAD" Branch
- The single currently "active" or "checked out" commit.
  - i.e. the commit that you are working on.
- **Detached HEAD**
  - Detached HEAD refers to the HEAD pointing to a specific commit rather than a branch.
  - Example
    - `HEAD` &rarr; `main` &rarr; `commit-hash`
    - `git checkout commit-hash`
    - `HEAD` &rarr; `commit-hash` (Detached HEAD)
#### Relative Refs
- `^` - move one commit upwards at a time.
  - i.e. parent commit of specified commit.
  - Ex: `main^`, `main^^`
- `~<num>` - move `nums` number of commits upwards.
  - Convenient to reassign a branch to a commit.
    - Ex: `git branch -f main HEAD~3` moves the main branch three commits behind HEAD.
#### Local Branches vs Remote Branches
- Most of the times we are working with the Local Branches as we are mostly working on our local machines.
- **Remote Branch**
  - Remote Branches reflect the state of Remote Repositories (at the point of your last "contact" with the remote repository).
  - `<remote-name>/<branch-name>`
    - Ex: `origin/main` is a remote branch in the .git directory in local that reflects the main branch of the remote repository called origin.
  - Checking out a Remote Branch results to detached HEAD since you cannot work on these branches directly.
    - A Remote Branch can only update when its remote updates.
    - Ex: `git checkout origin/main`, then `git commit` does not update `origin/main`. Instead, it puts HEAD into detached state.
  - `git fetch <remote-name> <branch-name>` updates a remote branch to reflect its corresponding branch in remote.
  - _Note: Remote branches are on local repository, not on the remote repository._
### Branch Commands
#### Creating a New Branch
- **`git branch <new-branch-name>`**
  - Git will start the new branch based on the currently checked out revision (the situation that I was to this point).
  - **`git branch <new-branch-name> <revision-hash-of-a-particular-commit>`**
    - Starts a branch on the specified revision.
- **`git switch -c <new-branch-name>`**
  - Creates a new branch and switches to it.
- **`git checkout -b <new-branch-name>`**
  - Creates a new branch and switches to it.
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
#### Moving Branches
- **`git branch -f <target-branch> <destination-commit>`**
  - Move `<target-branch>` to `<destination-commit>`.
- **`git reset <destination-commit>`**
  - Moves the HEAD branch to `<destination-commit>`.
  - _Only use this command to undo something._
#### Tracking Branches
- Connecting local and remote branches with each other.
  - Ex: you're trying to join in on a remote branch that someone else is working on.
- **`git branch --track <new-local-branch-name> origin/<base-branch>`**
  - Create a local branch with the specified base remote branch.
  - Ex: `git branch --track feature/login origin/feature/login`
- **`git checkout --track origin<base-branch>`**
  - If a local branch name to use is not specified, Git automatically uses the name of the remote branch for the local.
- **`git branch -vv`**
  - Show all tracking branches (configured for `git pull`).
- **`git branch -v`**
  - Shows any discrepancies in commits between local and remote.
  - Ex: ahead 1, behind 2.
#### Pushing and Pulling Branches
- Synchronizing your local and remote branches.
- **`git push <remote-name> <branch-name>`**
  - Upload local branch to the remote repository.
  - **`-u`** or **`--set-upstream`**
    - Sets the default remote branch for your current local branch to publish to.
    - Henceforth, `git push` defaults to pushing to that remote branch.
    - Ex: `git push -u origin feature/navbar`.
      - Push up the current HEAD branch to the feature/navbar branch on origin.
    - If the branch is only on local yet, use this command to upload the branch to GitHub and simultaneously set that remote branch as the up stream for this branch.
- **`git pull`**
  - Download new commits from the remote.
  - Ex: `git pull origin main`
#### Deleting Branches
- **`git branch -d <branch-name>`**
  - Delete a branch in your local repository. 
  - *Cannot delete the HEAD branch! So switch to another branch, then delete.*
- **`git push <remote-name> --delete <branch-name>`**
  - Delete a branch in the remote repository.
  - _Might also make sense to delete other branches that are tracking that remote branch!_
#### Combining Branches
##### Merging Branches
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Version%20Control%20System/refImg/git-merge.png" alt="Git Merge" width="60%"/>
</p>

- Integrating changes (bringing new commits) from another branch *into* your current local HEAD branch.
- _Merging creates a merge commit (the "melting point")._
- Merging branches can often lead to [merge conflicts](https://github.com/Kakamotobi/Learned/main/Version%20Control%20System/Version-Control-System.md#merge-conflicts).
- **2 Step Process**
  - **`git switch <branch-name>`**
    - Switch to the branch that you want to _receive_ the changes.
  - **`git merge <source-branch>`**
    - Add the `<source-branch>`'s commits into the HEAD branch.
      - _It does not mean that the branch and the HEAD are now in sync. They are still separate branches._
      - Ex: if `feature/navbar` points to `C5`, and `main` points to `C6`, `git merge feature/navbar` means `main` points to `C7` (the merge commit) and `feature/navbar` remains pointing to `C5`.
- _Note_
  - When using `git merge`, if you merge the `main` branch into your `feature` branch in order to keep on track with other changes to the `main` branch, you will create a merge commit on your `feature` branch.
    - If the `main` branch is very active, your `feature` branch will end up with lots of merge commits, resulting to a messy branch history.
    - That goes the same for your colleagues. Therefore, when everyone merges back to the master branch, the master branch will have a bunch of non-informative merge commits.
    - Therefore, it may be better to use `rebase` instead.
##### Rebasing Branches
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Version%20Control%20System/refImg/git-rebase.png" alt="Git Merge" width="60%"/>
</p>

- An alternative way to integrate changes (cf. `git merge`) from the specified branch _into_ your HEAD branch.
- **`git rebase <base-branch-name> [<target-branch-name>]`**
  - The HEAD's (or the `<target-branch-name>`'s) current commit (and its history) is copied. These copied commits are added to `<base-branch-name>` so that `<base-branch-name>` becomes the base/parent branch.
    - _At this point, the HEAD points to the latest copied commit, and the `<base-branch-name>` branch remains pointing to the same commit (behind the HEAD)._
      - Ex: `git switch feature/navbar`, then `git rebase main`.
    - Make sure to update the `<base-branch-name>` pointer as well to the latest commit.
      - Ex: `git switch main`, then `git rebase feature/navbar`.
  - The commit that HEAD was previously pointing to still exists after rebasing.
  - There is no separate merge commit that will be created. So, development history appears to have happened in a straight line.
  - Rebasing can lead to [merge conflicts](https://github.com/Kakamotobi/Learned/main/Version%20Control%20System/Version-Control-System.md#merge-conflicts).
    - Resolve the conflict.
    - Then, stage file(s) with `git add` and continue rebasing with `git rebase --continue`.
  - **`git rebase -i <base-branch-name> [<target-branch-name>]`**
    - Shows which commits are about to be copied below the rebase target branch.
    - Useful for when you are unsure which commits you want.
    - Useful for reordering commits.
    - cf. **`git cherry-pick <commit(s)>`**
      - Copy the specified commit(s) and add onto the current HEAD.
      - Useful for when you know which commits you want.
###### When Not to Rebase
- Never rebase commits that have already been shared with others.
  - If your collaborators have the changes you have made on their machine, if you rebase, the same changes will have different commits. This makes it difficult to reconcile the alternate histories.
    - i.e. since you rebased, they will have commits that do not exist in yours, and vice versa.
#### Comparing Branches
- Checking which commits are in branch-B, but not in branch-A.
- **`git log [<branch-A>..<branch-B>]`**
  - Ex: `git log main..feature/uploader`
  - Ex: `git log origin/main..main`
  - `git log --follow <file>`
    - Check the commits of this file.
  - `git log --graph`
    - Shows a graphic overview of the git tree.

## Git Remote
- A **remote** is a duplicate instance of the repository that lives on a remote server.
- Git remote is really about transferring data to and from a remote repository.
- A single repository can have multiple remotes.
  - Ex: GitHub, Heroku, AWS, etc.
- **`git remote`**
  - Lists all the remotes associated with the local repository.
  - `git remote -v`
    - Reveals the particular URLs of each remote.
  - `git remote show <remote-name>`
    - Reveals more information about the particular remote.
- If initializing a new git repository on local, you need to register a remote to push commits to.
  - `git remote add <remote-name> <remote-repository-url>`.
### Remote Tracking Branch
- A reference/pointer (`<remote-name>/<branch-name>`) to the state of the master branch on the remote on local.
  - Ex: `origin/master`.
- It cannot be moved by myself. It points to the last known commit on the master branch on origin at the time of cloning.
- `git branch -r`
  - View the remote branchs that your local repository knows about.
- You can checkout remote branch pointers, which results a detatched HEAD.
  - Ex: `git checkout origin/master`.
### Upstream
- An **Upstream** refers to the default remote branch of the current local branch.
- Ways that an Upstream can be set for a Branch.
  - **`git push -u <remote-name> <branch-name>`**
    - Set the upstream as you push a newly created local branch to remote.
    - Ex: `git push -u origin feature/navbar`
  - **`git branch --set-upstream-to=<remote-name/branch-name>`**
    - If `<branch-name>` is already on remote repo.
- **`git remote add <remote-name> <upstream-repo-url>`**
  - Registering a new remote repo to your local Git repository.
  - i.e. setting up an alias `<remote-name>` to `git push` to and `git pull` from.
  - Ex: `git remote add upstream https://github.com/some-upstream-repo.git`
  - Ex: `git remote add origin https://github.com/some-remote-repo.git`

## Git Clone and Git Fork
- The difference comes down to how much control a developer has over a given repository.
### Git Clone
- **`git clone <git-URL>`**
  - Clone the repository at the specified URL.
  - Only clones the default branch.
    - Other existing branches will not be cloned.
    - However, your local repository knows about them as information about them is downloaded.
      - `git branch -r` reveals them.
    - *Simply do `git switch <branch-name>` to create a new local branch from the remote branch of the same name.*
      - Ex: if remote has a branch called "chickens" (but it is not on local), `git switch chickens` will make a local "chickens" branch and set it up to track the remote branch "origin/chickens".
      - You can also do `git checkout --track <remote-name>/<branch-name>`.
- Changes can be made to this cloned version in local.
- Only designated contributors are allowed to push back to repository on origin.
  - Undesignated contributors' attempt to push will lead to a 403 error.
  - If you're a collaborator, make sure to clone the repository and not fork it, as you would want to collaborate on the same GitHub repository with your teammates.
### Git Fork
- Anyone can fork any repository on GitHub.
  - There is no Git command for forking (use GitHub).
- The repository is completely copied to your own GitHub as a separate entity to the original.
  - There is an option to include/exclude all branches aside from main.
- Check [collaboration workflow](https://github.com/Kakamotobi/Learned/main/Version%20Control%20System/Version-Control-System.md#fork--clone-workflow).

## Pull Request
- **A request that your changes in a branch be pulled in and merged to a particular branch.**
- Instead of everyone merging to the master branch at free will, pull requests act as a mechanism to intermediate changes ready to be integrated to the master branch.
  - They help facilitate discussion and feedback on the specified commits.
- Pull requests on a given branch can be approved or rejected.
- The master branch can be protected by making pull requests mandatory for it.
### How it Works
- **`<Base Repository>:<Base Ref> <- <Head Repository>:<Head Ref>`**
  - Request to merge the head ref/branch on head repository into the base ref/branch on base repository.
  - `Base Repository` - the upstream repository.
  - `Base Ref` - the branch you wish to merge into.
  - `Head Repository` - the source repository.
  - `Head Ref` - the branch with the changes that you wish to merge to upstream.
### Workflow - only local and origin
- You have done some work locally on a feature branch.
- You commit your changes and then push the feature branch to origin (GitHub).
- Then, open a pull request using the feature branch that you just pushed up to origin.
  - On the repository's feature branch's GitHub page, click on "Compare & pull request" or "Pull request".
  - Describe the pull request and create pull request.
    - Ex: what branch (usually master) to merge the feature branch to.
- Wait for the PR to be approved and merged by the reviewer (in charge of merging).
- If more changes need to be made, simply add more commits to your feature branch and push it to origin again.
  - The PR will update automatically to reflect your changes.
### Important Notes
- *By the time your PR is accepted, there may be a newer version of the master branch. Therefore, your changes may not work on the newer version of master.*
  - Take one of two options to get your feature branch up to date:
    - `git merge master`: applies any new changes from the master branch on top of your work.
    - `git rebase master`: re-applies your work on top of any new changes on the trunk.
    - In both options, your own changes will remain the same as you are essentially just moving your branch up to the top of the trunk to stay up to date with the newest version.
    - It is usually safer to `merge`.
- *If merge conflicts occur after attempting a PR, simply resolve them and continue.*
  - As a reviewer (or any contributor), download the latest changes, switch to that feature branch, merge the master branch into the feature branch.
    - Ex: `git fetch origin` &rarr; `git switch feature-branch` &rarr; `git merge master`.
  - Resolve the conflict.
  - Then, switch to the master branch, merge the feature branch into the master branch, commit, and push to remote.
    - `git switch master` &rarr; `git merge --no-ff feature-branch` &rarr; `git commit -am "commit message"` &rarr; `git push origin master`.

## Merge Conflicts
- **When you made changes to a line of code that someone else has also made changes to, Git is in a conflict as to which version to keep.**
  - The notorious merge conflict output: `>>>>>>> ======= <<<<<<<`
  - ***Once the merge conflict is taken care of, git performs a "merge commit", which is a new commit on the HEAD branch.***
    - *This merge commit has two parent commits (one from the other branch, one from Head).*
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

## Git Collaboration Workflows
### Centralized Workflow
- The most basic workflow where everyone works on the master branch.
- It can work for very small teams but it has some shortcomings.
  - If someone has updated master on remote, you will not be able to push because "the tip of your current branch is behind its remote counterpart". Therefore, you will have to merge the remote changes (Ex: `git pull`) before pushing.
  - The only way to share code (for revision purposes, etc.) is to push a commit to master on remote.
    - However, if that code is broken, the master on remote and everyone's code will be broken.
### Feature Branch Workflow
- No one works on the master branch.
- All new development should be done on separate branches. Therefore, the master branch will not contain broken code.
- The master branch is treated as the official project history.
- Teammates can collaborate on a single feature and share code back and forth without polluting the master branch.
### Fork & Clone Workflow
- Instead of everyone working on one centralized GitHub repository, every developer has their own GitHub repository in addition to the "main" repository.
- Developers make changes and push to their own forks efore making pull requests to the "main" repo.
- This method allows for us to contribute to a project that we do not own.
#### Flow
1) Fork a repository on GitHub.
2) Clone the forked repository to local.
    - Ex: `git clone <forked-repo-git-url>`.
3) Add another remote pointing to the original project repository (not the fork in your repository).
    - Often named "upstream" or "original".
    - *This is so that you can pull new changes from it (since your fork repository will not have those new changes made in the original repository).*
    - Ex: `git remote add upstream <original-repo-git-url>`.
4) Make changes and commit them.
    - Ex: create new branch `git branch feature/navbar` in local.
5) Push the working branch to the repository on origin (your forked repository. Not the original repository that was forked from).
    - Ex: `git push origin feature/navbar`.
6) Make a **pull request** from your forked repository (on origin) to the original repository.
    - On GitHub, go to your forked repository and click "New pull request" and then "Create pull request".
      - Ex: `upstream:landing-page <- forked-repo:feature/navbar`
    - This sends a message to the original repository asking them to "pull" the changes that have been made.
    - The receiver (original repository) will see the pull request and can decide whether or not to accept the changes made and "Merge pull request".
6) If your pull request requires changes and is not accepted yet, leave the pull request open and keep pushing commits to the working branch.
7) If your pull request is accepted, synchronize the original repository and local by pulling down the (your) approved changes on the original repository (upstream remote) to your local machine.
    - Ex: `git fetch upstream landing-page` and `git rebase upstream/landing-page`.
    - Ex: `git pull upstream landing-page`.
8) Push to the repository on origin (your forked repository. Not the original repository that was forked from).
    - Ex: `git push origin landing-page`.
9) If your pull request was accepted, it is now safe to delete the branch that you've been working on in local (and the corresponding branch in origin).
    - Ex: `git branch -d feature/navbar`
10) Make more commits, and push to forked repository (origin remote), and repeat the PR process (starting from step 4).

## Frequently Used Git Commands
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
- **`git rm <filename>`**
  - Remove a file(s) from the working tree and from the index.
  - `git rm --cached <filename>`
    - Unstage and remove paths only from the index, and leave the working tree files untouched.
- **`git mv <source> <destination>`**
  - Rename or move a file, symlink or directory.
- **`git status`**
  - Check the Git status of files.
    - Ex: what files have been modified.
- **`git diff`**
  - Prints the difference between the current working tree and the record in the index.
    - i.e. prints a list of the changes in the working directory that are ***not staged*** for the next commit.
  - It is a good way to double-check before committing.
  - `git diff HEAD`
    - List all changes in the working directory since the last commit.
  - `git diff --staged` or `git diff --cached`
    - List all the changes that will be included in the commit if you run `git commit` right now.
  - `git diff HEAD <filename>`, `git diff --staged <filename>`
    - List all changes in the specific file.
  - `git diff <branch-name> <other-branch-name>`
    - Prints the differences between two branches.
  - Output Example
    ```zsh
    diff --git a/main.txt b/main.txt # Compared files (a being the file before changes, b being the file after changes)
    index 72d1d6a..f2c8117 100644 # File meta data
    --- a/main.txt # Indicate changes
    +++ b/main.txt # Indicate changes
    @@ -3,4 +3, 5 @@ orange # Chunks that were modified (incl. some unchanged lines before and after)
    yellow
    green
    blue
    -purple # From file a
    +indigo # From file b
    +violet # From file b
    ```
- **`git add <file(s)>`**
  - Move file(s) to the Staging Area from the Working Directory (after modification).
  - `git add --all` moves all files including untracked ones.
- **`git commit`**
  - Record changes to the repository.
  - A commit is a type of checkpoint called a "revision", and is a hash of numbers and letters.
  - Ex: `git commit -m "Fix bug."`
- **`git push <remote-name> <branch-name>`**
  - Updates remote refs using local refs, while sending objects necessary to complete the given refs.
  - i.e., update remote repo with changes made in local repo.
  - Ex: `git push origin main` pushes changes to the main branch on the remote origin.
  - Alternatively, **`git push <remote-name> HEAD`** can be used.
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
    ```zsh
    git log // find the commit id you want.
    git checkout <commitId> . // the "." refers to the current directory.
    git commit -m "Restoring old source code" // make a new commit with the restored one.
    ```
- **`git show [<commit>]`**
  - Show object(s) representing commits and meta data.
  - `git show-ref`
    - List the references in the local repository.
- **`git fetch <remote-name> <branch-name>`**
  - Download all changes from a remote repository and update the remote branch (Ex: origin/main) in local.
    - Those changes are not yet integrated into the working directory.
    - i.e. synchronize your local representation of the remote repository with the actual remote repository.
  - _It is a way of fetching and accessing the latest changes to the repository on your machine BUT without having to actually merge them into our working files._
    - "Go and get the latest information from GitHub. But don't modify my working directory."
  - Ex: `git fetch origin` will fetch all changes (incl. new branches that have been created) from the origin remote repository.
  - Ex: `git fetch origin master` will fetch all changes from the master branch on the origin remote repository.
  - Example
    - You made changes to the master branch on local.
    - But the remote repository has changed because other people have pushed up changes to the master branch.
    - `git fetch origin master` will fetch those changes.
      - ***This updates the `origin/master` remote tracking pointer but your master branch on local is untouched.***
    - `git checkout origin/master` to see them.
- **`git merge <source-branch-name>`**
  - `git merge` updates your current branch with whatever changes are on the specified source branch.
  - Takes all the commits existing on the `source-branch` branch and integrate them ***into*** your current branch.
  - Since this uses whatever branch data that is stored locally at the moment, run `git fetch` first to download the latest information before merging.
  - Ex: if another developer added some commits to the `master` branch of `origin`, checkout the `master` branch in local, download their changes using `git fetch` and effectively update the `master` branch in local. Then, checkout the target branch in local, and `git merge origin/master` to merge the origin/master branch into your current branch.
    - _`origin/master` refers to the `origin/master` checkpoint in local. This notation is used to differentiate branches of the same name (Ex: `master`) located in different places (Ex: your own branches vs. origin's branches)._
- **`git pull <remote-name> <branch-name>`**
  - Download changes from a remote repository. And update your HEAD branch with whatever changes are retrieved from the remote.
  - "Go and get the latest information from GitHub. And immediately update my local repository with those changes.
  - `git pull` = `git fetch` + `git merge`.
  - ***The current HEAD branch is where the changes will be pulled down to.***
    - Ex: if on the master branch, `git pull origin master` will fetch the latest information from the origin's master branch and merge those changes into the master branch on local.
  - `git pull` defaults to remote being 'origin' and branch being the tracking connection that is configured for your current branch.
    - Ex: `git pull` on master branch on local will pull from origin/master.
    - Ex: `git pull` on 'chickens' branch on local will pull from origin/chickens.
  - *Make sure to do this (update your repo with the latest version) before pushing to GitHub or creating a new branch to work on.*
  - Pulls can result in merge conflicts.
  - `git pull origin master` vs `git pull origin/master`
    - `git pull origin master` will pull changes from the `master` branch on the `origin` remote and merge that to the local HEAD branch.
    - `git pull origin/master` will pull changes from the `origin/master` tracking branch on **local** and merge that to the local HEAD branch.
  - cf. **`git pull --rebase`**
    - Fetch updates on the remote repository to the remote branch, and rebase (instead of merge) HEAD to the updated remote branch.
    - A way to update your work to incorporate updates that have been made in the meantime to the remote repository.

## Reference
[File system - Wikipedia](https://en.wikipedia.org/wiki/File_system)  
[Distributed version control - Wikipedia](https://en.wikipedia.org/wiki/Distributed_version_control)  
[Git - Book](https://git-scm.com/book/en/v2)  
[Git - Reference](https://git-scm.com/docs)  
[gitcore-tutorial(7)](https://git.github.io/htmldocs/gitcore-tutorial.html)  
[giteveryday(7)](https://git.github.io/htmldocs/giteveryday.html)  
[Changing a commit message - GitHub Docs](https://docs.github.com/en/pull-requests/committing-changes-to-your-project/creating-and-editing-commits/changing-a-commit-message)  
[Git Branch | Atlassian Git Tutorial](https://www.atlassian.com/git/tutorials/using-branches)  
[깃, 깃허브 제대로 배우기 - YouTube](https://www.youtube.com/watch?v=Z9dvM7qgN9s&ab_channel=%EB%93%9C%EB%A6%BC%EC%BD%94%EB%94%A9by%EC%97%98%EB%A6%AC)  
[Git Branches Tutorial](https://www.youtube.com/watch?v=e2IbNHi4uCI&ab_channel=freeCodeCamp.org)  
[The Ultimate GitHub Collaboration Guide | by Jonathan Mines](https://medium.com/@jonathanmines/the-ultimate-github-collaboration-guide-df816e98fb67)  
[Understanding Git (part 1) - Explain it Like I’m Five | HackerNoon](https://hackernoon.com/understanding-git-2-81feb12b8b26)  
[Understanding Git (part 2) - Contributing to a Team | HackerNoon](https://hackernoon.com/understanding-git-2-81feb12b8b26)  
[Git Fork vs. Git Clone: What's the Difference? - YouTube](https://www.youtube.com/watch?v=6YQxkxw8nhE&ab_channel=EyeonTech)  
[Git Detached Head: What Is It & How to Recover](https://www.cloudbees.com/blog/git-detached-head)  
[git-cheat-sheet-education](https://education.github.com/git-cheat-sheet-education.pdf)  
[Learn Git Branching](https://learngitbranching.js.org/)  
