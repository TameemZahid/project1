# Git Basics 
Git is a distributed version control system that allows multiple developers to work on a project simultaneously. Below are some fundamental Git operations and commands.

## Creating Personal access tokens
Personal access tokens are used to authenticate against Git over HTTP. They are the only accepted password when we have Two-Factor Authentication (2FA) enabled.

## Configuring Git
Before using Git, set up your user information:
```
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

## Cloning a Repository
To clone a repository from a remote source (like GitHub, GitLab):

```
git clone <repository_url>
```

Example:

```
git clone https://github.com/user/repo.git
```

## Checking Repository Status
To check the status of your repository:
```
git status
```

## Pulling Latest Changes
To update your local repository with the latest changes from the remote repository:
```
git pull origin <branch_name>
```
Example:
```
git pull origin main
```
## Creating a New Branch
To create a new branch and switch to it:

```
git checkout -b <new_branch_name>
```
Example:
```
git checkout -b feature-branch
```

## Switching Branches
To switch between branches:
```
git checkout <branch_name>
```
Example:
```
git checkout main
```

## Adding and Committing Changes
To stage and commit changes:
```
git add <file_name> # Add a specific file
git add . # Add all changes

git commit -m "Your commit message"
```

## Pushing Changes to Remote Repository
To push changes to the remote repository:
```
git push origin <branch_name>
```
**Note:** While pushing the changes to remote repo, prompt may ask for password/PAN, we need to provide this PAN here, then changes will be pushed to remote repo. 

Example:
```
git push origin feature-branch
```

## Merging Branches
To merge another branch into the current branch:
```
git merge <branch_name>
```

Example:
```
git merge feature-branch
```

## Viewing Commit History
To see commit history:

```
git log --oneline --graph --all
```

## Resetting Changes
To undo changes before committing:
```
git checkout -- <file_name>
```

To reset a commit:
```
git reset --soft HEAD~1 # Keep changes in working directory
git reset --hard HEAD~1 # Remove changes permanently
```



 