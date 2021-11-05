# Git
Git is an open source version control tool that enables one to easily track changes and coordinate project works. 
Git supports non-linear development allowing multiple sections of the project to be amended at the same time.
Git works on commandline and it can be used with local or remote repositories. Github is one of popular Git repository server service. 

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
\$ git config --global user.name "Your name" <br>
\$ git config --global user.email "Your email"<br>
\*There are 3 levels in configuration:
- System: on all users
- Global: on current user for all repositories
- Currnet: on current repository

### Default Editor Setup
Default editor can be defined as below.<br>
\$ git config --global core.editor "code --wait"<br>
\*code refers to Visual Studio Code
\*--wait makes the git bash to wait while the editor is open

Files can be opened using the editor.
\$ git config --global -e

### End of Line Setup
Windows OS and Mac OS mark the end of line differently. 

On Windows, the end of line is marked with carriage return & line feed: \r & \n <br>
Whereas on Mac, the end of line is marked with only line feed: \n

This difference can be handled by Git by setting autocrlf.<br>
\$ git config --global core.autocrlf true<br>
\*On Mac, input is used instead of true

## Initialising 
To create a new repository, Git must be initialised in the working directory. <br>
\$ git init

## Staging Files
Files can be added to Staging Area to let Git to track the changes made. <br>
\$ git add "File 1" "File 2" -> Specific files can be added together. <br>
\$ git add . -> Adds all files in the working directory. <br>
\$ git add \*.txt -> Adds all .txt files. <br>
\$ git rest "file 1" "File 2" -> To unstage files

### Check Staging Files
Files on the staging area can be viewed using. <br>
\$ git ls-files

### Removing File from Staging Area & Working Directory
Files that are committed can be removed from the staging area and working direcotry. <br>
Note that the file in either working directory or staging area must be identical to the committed version. <br>
\$ git rm \<file name\> <br>

## Checking Git Status
Status shows files that are currently untracked (Working Tree) and tracked (Staging Area). <br>
\$ git status <br>
\$ git status -s -> gives shortened status 
  
## Making Commits
Tracked files in Staging Area can be committed with the following command. <br>
\$ git commit -m "Commit message"

To edit the message on the editor, run the follow. <br>
$ git commit <br>
\*First line is for short description < 80 characters. And long description can be made after a line spacing. <br>

### Commit Practice
- Commit often and make each commit logical and meaningful with comments
- For commenting, stick to Present tense or Past tense

### Commit History
Recent commit history can be checked. And each history log will have commit hash. <br>
$ git log <br>
$ git log --online -> to show each history in one line <br>
$ git log --online --reverse -> from beggining till end <br>

Textual difference of the commit log can be viewed by: <br>
$ git show \<commit hash\>

And file tree at that commit can be viewed by: <br>
$ git ls-tree \<commit hash\> <br>
\* This will show file has, and you can 'git show \<file hash\>' to view the content at that commit snapshot.

## Checking Difference
In addition to Git status, the following code shows all changes made to the files.<br>
$ git diff <br> -> Comparison between working directory & staging area
$ git diff --staged -> Comparison between staging area & repostitory

### Visually Checking Changes
Visual diff tool can be used. In this case, VS code will be used.

Firstly, it needs to be defined in the config file. <br>
$ git config --global diff.tool vscode -> defining diff tool's name <br>
$ git config --global difftool.vscode.cmd "code --wait --diff $LOCAL $REMOTE" -> defining how diff tool, vscode can be launched <br>
\*--diff $LOCAL $REMOTE is for comparison with placeholder variables
\* Run 'git config --global -e' to ensure the config setting is set correctly.

## Restore Staged File or Committed File
$ git restore \<file name\> -> gets a copy of the file in the staging area and overwrite on the file in the working direcotry (restore from staging area) <br>
$ git restore --staged \<file name\> -> gets a copy of the file in the repository and overwrite on the file in the staging area (restore from repository) <br>

## Restore File from Commit Logs
A file can be restored from the commit versions. <br>
1. $ git log --oneline -> to look for committed version of interest
2. $ git ls-tree \<commit hash\> -> to check if the file is in the commit version
3. $ git restore --source=\<commit hash\> \<file name\> -> to restore the file from the specified commit version

## Restore Working Tree (Switching Branch)
Files in Working Tree can be restored to the version previously committed. This will create a new branch. <br>
On the other hand, branch can be directly given to change the files in Working Tree. <br>
$ git checkout \<Commit hash\> -> When restoring to previously committed version. Commit hash can be checked in Git log. <br>
$ git checkout \<Branch name\> -> When switching to other branch. <br>
$ git checkout master -> To go back to the main branch. <br>
$ git branch -> Shows available branches.

## Creating Branch
Branch is an individual environment separeted from the main branch. Branches can coexist at the same time.
Bracnhes are often used as a platform to edit codes or experiment features before merging the changes to the main branch. <br>
When changes are committed while on a branch, the committed changes do not affect other branches. <br>
$ git branch \<Branch name\>

## Merging Branches
Branches can be merged by implementing code changed of another branch to the current branch. <br>
$ git merge \<Branch name\>

### Merge Conflict
When changes are made at the same section of the code from both branches, merge conflict occurs. In this case, Git
can accept current change, incoming change, both changes or abort merge. <br>
It is recommended to make frequent small commits to avoid conflicts or minimise conflicting area.

## Deleting Branch
Branch can be deleted by using the following command. <br>
$ git branch -d \<Branch name\>

## Stashing Files
Files and changes that are not ready to be committed can be stashed (saved temporarily) in local Git repository. 
This enables one to work on different branches and come back to the stashed work. <br>
$ git stash -> Stashes tracked changes (for files that have been added to Staging Area) <br>
$ git stash -u -> Stashes all files (for files in both Working Tree and Staging Area) <br>
$ git stash pop -> Load stashed files and changes

## Ignore Files
Files can be explicitly ignore by Git. These files can be dependency caches or personal IDE config files. <br>
$ touch .gitignore -> create .gitignore file <br>
$ code .gitignore -> edit file using VS code <br>
\*File names or patterns can be added on each line. <br>
  
    logs/
    main.log
    *.log

## Pushing to Github Repository 
In order to push files and changes to Github repository, the local Git repository must be cloned from Github repository. <br>
$ git clone <Github repo address> <br>
After cloning Github repository, files and changes can be pushed after they are committed to the local repository. <br>
$ git push origin master -> Pushes master brach (local) to origin of remote repository. <br>
Note that it's always good practice to do Git pull beforehand to ensure the local files are in sync to the remote repo. <br>
$ git pull origin <Branch name> -> Branch name to pull remote repo into.


