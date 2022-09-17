# Git
Git is an open source version control tool that enables one to easily track changes and coordinate project works. 
Git supports non-linear development allowing multiple sections of the project to be amended at the same time.
Git works on commandline and it can be used with local or remote repositories. Github is one of popular Git repository server service. 

## Table of Contents
- [Structure](#structure)
  - [Working Tree](#working-tree)
  - [Staging Area](#staging-area)
  - [Repository](#repository)
- [Installation](#installation)
- [Configuration](#configuration)
  - [Current User Setup](#current-user-setup)
  - [Default Editor Setup](#default-editor-setup)
  - [End of Line Setup](#end-of-line-setup)
- [Initialising](#initialising)
- [Staging Files](#staging-files)
  - [Check Staging Files](#check-staging-files)
  - [Removing File from Staging Area & Working Directory](#removing-file-from-staging-area--working-directory)
- [Checking Git Status](#checking-git-status)
- [Making Commits](#making-commits)
  - [Commit Practice](#commit-practice)
  - [Commit History](#commit-history)
  - [Rollback/Revert to specific commit history](#RollbackRevert-to-specific-commit-history)
- [Checking Difference](#checking-difference)
  - [Visually Checking Changes](#visually-checking-changes)
- [Restore Staged File or Committed File](#restore-staged-file-or-committed-file)
- [Restore File from Commit Logs](#restore-file-from-commit-logs)
- [Restore Working Tree (Switching Branch)](#restore-working-tree-switching-branch)
- [Creating Branch](#creating-branch)
- [Merging Branches](#merging-branches)
  - [Merge Conflict](#merge-conflict)
- [Deleting Branch](#deleting-branch)
- [Stashing Files](#stashing-files)
- [Ignore Files](#ignore-files)
- [Remote Repository](#remote-repository)

## Structure
Git consists of 3 areas. <br>
<img src="https://github.com/TravisH0301/learning/blob/master/images/git_structure.png" width="400">

### Working Tree
Working Tree contains the files that are being worked on. This area is also known as Untracked area and 
any changes made to the files will be marked yet will not be tracked nor saved by Git. 
In order to let Git to track the changes, the files must be added to be Staging Area.

### Staging Area
Staging Area is where Git starts to track changes made to the files. Yet, any additional changes
made to the files after adding them to Staging Area will require one to re-add the files to Staging Area
to let Git to save the new changes. In this area, the tracked changes of the files can be committed to 
Repository. <br>
Note that only files that have been added to Staging Area or been committed will be tracked by Git. 

### Repository
Repository is where all committed files and their tracked changes are stored. 

## Installation
Git can be downloaded from its official website. <br>
[Git Download](https://git-scm.com/downloads)

Git bash will be used for any examples below. 

## Configuration
### Current User Setup
This configuration enables identification of any changes made by the user. <br>

    $ git config --global user.name "Your name"
    $ git config --global user.email "Your email"

\*There are 3 levels in configuration:
- System: on all users
- Global: on current user for all repositories
- Currnet: on current repository

### Default Editor Setup
Default editor can be defined as below.<br>

    $ git config --global core.editor "code --wait"

\*code refers to Visual Studio Code
\*--wait makes the git bash to wait while the editor is open

Files can be opened using the editor.
    
    $ git config --global -e

### End of Line Setup
Windows OS and Mac OS mark the end of line differently. 

On Windows, the end of line is marked with carriage return & line feed: \r & \n <br>
Whereas on Mac, the end of line is marked with only line feed: \n

This difference can be handled by Git by setting autocrlf.<br>

    $ git config --global core.autocrlf true

\*On Mac, input is used instead of true

## Initialising 
To create a new repository, Git must be initialised in the working directory. <br>

    $ git init

## Staging Files
Files can be added to Staging Area to let Git to track the changes made. <br>
    
    $ git add "File 1" "File 2" -> Specific files can be added together.
    $ git add . -> Adds all files in the working directory.
    $ git add *.txt -> Adds all .txt files.
    $ git reset "file 1" "File 2" -> To unstage files

### Check Staging Files
Files on the staging area can be viewed using. <br>

    $ git ls-files

### Removing File from Staging Area & Working Directory
Files that are committed can be removed from the staging area and working direcotry. <br>
Note that the file in either working directory or staging area must be identical to the committed version. <br>
    
    $ git rm <file name>

## Checking Git Status
Status shows files that are currently untracked (Working Tree) and tracked (Staging Area). <br>

    $ git status
    $ git status -s -> gives shortened status 
  
## Making Commits
Tracked files in Staging Area can be committed with the following command. <br>

    $ git commit -m "Commit message"

To edit the message on the editor, run the follow. <br>

    $ git commit

\*First line is for short description < 80 characters. And long description can be made after a line spacing. <br>

### Commit Practice
- Commit often and make each commit logical and meaningful with comments
- For commenting, stick to Present tense or Past tense

### Commit History
Recent commit history can be checked. And each history log will have commit hash. <br>

    $ git log
    $ git log --online -> to show each history in one line
    $ git log --online --reverse -> from beggining till end

Textual difference of the commit log can be viewed by: <br>

    $ git show <commit hash>

And file tree at that commit can be viewed by: <br>

    $ git ls-tree <commit hash> 

\* This will show file has, and you can 'git show \<file hash\>' to view the content at that commit snapshot.

### Rollback/Revert to specific commit history
This code will rollback to the stage where the specified commit was just made.

    $ git revert --no-commit <commit hash ex)0766c053>..HEAD
    $ git commit 

## Checking Difference
In addition to Git status, the following code shows all changes made to the files.<br>

    $ git diff -> Comparison between working directory & staging area
    $ git diff --staged -> Comparison between staging area & repostitory

### Visually Checking Changes
Visual diff tool can be used. In this case, VS code will be used.

Firstly, it needs to be defined in the config file. <br>

    $ git config --global diff.tool vscode -> defining diff tool's name
    $ git config --global difftool.vscode.cmd "code --wait --diff $LOCAL $REMOTE" -> defining how diff tool, vscode can be launched

\*--diff $LOCAL $REMOTE is for comparison with placeholder variables
\* Run 'git config --global -e' to ensure the config setting is set correctly.

## Restore Staged File or Committed File

    $ git restore <file name> -> gets a copy of the file in the staging area and overwrite on the file in the working direcotry (restore from staging area)
    $ git restore --staged <file name> -> gets a copy of the file in the repository and overwrite on the file in the staging area (restore from repository)

## Restore File from Commit Logs
A file can be restored from the commit versions. <br>

    1. $ git log --oneline -> to look for committed version of interest
    2. $ git ls-tree <commit hash> -> to check if the file is in the commit version
    3. $ git restore --source=<commit hash> <file name> -> to restore the file from the specified commit version

## Restore Working Tree (Switching Branch)
Files in Working Tree can be restored to the version previously committed. This will create a new branch. <br>
On the other hand, branch can be directly given to change the files in Working Tree. <br>

    $ git checkout <Commit hash> -> When restoring to previously committed version. Commit hash can be checked in Git log.
    $ git checkout <Branch name> -> When switching to other branch.
    $ git checkout master -> To go back to the main branch.
    $ git branch -> Shows available branches.

## Creating Branch
Branch is an individual environment separeted from the main branch. Branches can coexist at the same time.
Bracnhes are often used as a platform to edit codes or experiment features before merging the changes to the main branch. <br>
When changes are committed while on a branch, the committed changes do not affect other branches. <br>

    $ git branch <Branch name>

## Merging Branches
Branches can be merged by implementing code changed of another branch to the current branch. <br>
    
    $ git merge <Branch name>

### Merge Conflict
When changes are made at the same section of the code from both branches, merge conflict occurs. In this case, Git
can accept current change, incoming change, both changes or abort merge. <br>
It is recommended to make frequent small commits to avoid conflicts or minimise conflicting area.

## Deleting Branch
Branch can be deleted by using the following command. <br>

    $ git branch -d <Branch name>

## Stashing Files
Files and changes that are not ready to be committed can be stashed (saved temporarily) in local Git repository. 
This enables one to work on different branches and come back to the stashed work. <br>

    $ git stash -> Stashes tracked changes (for files that have been added to Staging Area)
    $ git stash -u -> Stashes all files (for files in both Working Tree and Staging Area)
    $ git stash pop -> Load stashed files and changes

## Ignore Files
Files can be explicitly ignore by Git. These files can be dependency caches or personal IDE config files. <br>

    $ touch .gitignore -> create .gitignore file
    $ code .gitignore -> edit file using VS code

\*File names or patterns can be added on each line. <br>
  
    logs/
    main.log
    *.log

## Remote Repository
There 2 ways that a remote repository can be accessed from local machine.
- Cloning a remote repository to create a local repository<br>
  
  $ git clone <url> <directory(optional)><br>
  This command will download the files from the remote repository and create a local repository. By default the remote repository will
  become the origin remote repository that you can push to.<br>
  
  $ git clone -brand <branch_name> <url> <directory(options)><br>
  This command will download the specific branch of the remote repository. 

- Connecting an existing local repository to a remote repository<br>
  
  $ git remote add <name> <url>
  This command adds the remote repository with the specified name. Multiple remote repositories can be set and only one will become the main remote repository.<br>
  
  $ git remote -v<br>
  This command displays all available remote repositories with the URL.<br>
  
  $ git remote rm <name><br>
  $ git remote rename <name> <new_name><br>
  These commands can be used to amend remote repositories.
  
Git takes in both HTTP(Hyper Text Transfer Protocol) URL and SSH(Secure Shell) URL for connecting to remote repository.<br>
HTTP allows annoymous access to the remote repository, however, often it is not permitted to push annoymously.<br>
SSH provides authentication and allows more sure way of connecting to the remote repository.

### Updating Local Repository
There are 2 ways to update the local repository.<br>
- Fetch remote repository, view changes and merge<br>
  Fetch will download the commits and files, but will store them as reference withouth impacting the local repository.<br>
  
  $ git fetch <remote_repository_name> <remote_branch(optional)><br>
  This command will fetch the all branches (unless specified) of the remote repository.
  
  $ git branch -r<br>
  This command will display all references for the remote branches that are fetched.
  
  $ git checkout <fetched_remote_branch><br>
  This will lead to a detached HEAD state (meaning the current branch is separate from the existing local branch) and changes made
  in this fetched remote branch will not affect the existing local branch.<br>
  If a local branch is to be created based on this fetched remote branch, the following command can be used.<br>
  $ git checkout -b <now_loca_fetched_remote_branch>
  
  $ git log <local_branch>..<fetched_remote_branch> --oneline<br>
  This command will display all the commits made in the upstream branch.
  
  When merge is to be made, point to the local branch to be merged, and merge with the fetched remote branch.<br>
  $ git checkout <local_branch><br>
  $ git merge <fetched_remote_branch><br>
  Note that when these two branches have no common base, an error will occur: "refusing to merge unrelated histories"<br>
  This can be resolved by using the following parameter.<br>
  $ git merge <fetched_remote_branch> --allow-unrelated-histories<br>
  
- Pull remote repository while merging all contents automatically<br>
  Git pull is combination of git fetch and git merge. This is considered unsafe way that can put the current branch into a conflicted state.
  So careful execution is required.
  
  $ git pull <remote_repository><br>
  This command is equivalent to the following commands:<br>
  $ git fetch <remote_repository> HEAD & $git merge HEAD<br>
  HEAD referrs to the current branch.
  
### Pushing to Remote Repository

   $ git push <remote_name> <local_branch_name>
  
This command will push the changes in the local branch to the remote repository. By default when remote_name and local_branch_name are not given the changes made in the current local branch will be pushed to the origin remote repository. This behaviour can be configured.
  
    $ git push <remote_name> <local_branch_name>:<remote_branch_name>
  
By default, Git will push the changes in the specified local branch to the same remote branch. If you want to push the changes to the other remote repository branch, this command can be used. Also, it's a good idea to push to a new remote repository branch and create a pull request to make changes to the main remote branch while getting the changes checked by peers.
 
