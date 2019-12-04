
<p align="center">
    <img src="https://github.com/RORVI/cheatsheets/blob/master/Git/images/cheatsheet-title.png?raw=true" width="500" height="300">
</p>

![Project structure](https://github.com/RORVI/cheatsheets/blob/master/Git/images/project-structure.png?raw=true)

## A git project has three parts:
* **`working directory`**, which is the actual work directory, where you get work done
* **`staging area`**, which contains the list of changes made to the working directory
* **`repository`**, which is the place where your changes get stored into


![Commands](https://github.com/RORVI/cheatsheets/blob/master/Git/images/commands.png?raw=true)

**`git init`** -> used for initializing the repository locally

**`git status`** -> check the status of the changes you have made

**`git add .`** -> used for adding all files to the staging area ( basically to track changes )

**`git diff <filename>`** -> used for differentiating between what you have in the staging  regarding a file and what you have in the staging

**`git commit -am`**

**`git commit -m <message>`** -> used for committing changes from the staging area to the repository

**`git checkout <filename>`** -> used to discard changes in the working directory, ( sa zic doar de branch )

**`git reset <SHA>`** -> used to rewind everything up to the commit which has the first 7 characters of the 			  SHA code you enter

**`git branch`** -> used for finding out on which branch you're working on

**`git checkout -b <”branch_name”`** -> creates a new branch and moves me to the new branch

**`git branch <branch_name>`** -> used for creating a new branch, named "branch_name"

**`git checkout <branch_name>`** -> switch to the branch named "branch_name"

**`git merge <branch_name>`** -> used to merge another branch to the master branch, switch to the master branch and use the command ( or you can do it on Github )

**`git branch -d <branch_name>`** -> is used to delete a branch

**`git clone <remote_project> <local_project>`** -> used for making local repositories from an origin 

**`git remote update`** -> for checking if any changes have occurred in the origin while since you cloned 				into the local repository, and cloning the changes into your repository

**`git pull`**->used for syncing the local with the remote

**`git push origin <branch_name>`** -> is used for merging the local changes to the remote repository.


![Workflow](https://github.com/RORVI/cheatsheets/blob/master/Git/images/workflow.jpg?raw=true)
## The flow of working with Git:
1. **`Fetch and merge`** changes from the remote
1. **`Create a branch`** to work on a new project feature
1. **`Develop the feature on your branch`** and commit your work
1. **`Fetch and merge from the remote again`** (in case new commits were made while you were working)
1. **`Push your branch`** up to the remote for review
