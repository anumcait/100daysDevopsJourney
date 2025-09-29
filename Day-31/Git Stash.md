# Git Stash

## Problem Statement:

The Nautilus application development team was working on a git repository /usr/src/kodekloudrepos/games present on Storage server in Stratos DC. 

One of the developers stashed some in-progress changes in this repository, but now they want to restore some of the stashed changes. 


Find below more details to accomplish this task: Look for the stashed changes under /usr/src/kodekloudrepos/games git repository, and restore the stash with stash@{1} identifier. 

Further, commit and push your changes to the origin.

<img width="700" height="400" alt="image" src="https://github.com/user-attachments/assets/c7054649-f475-49e9-9f7f-3bc96005a235" />

## Solution

To accomplish the task of restoring the stashed changes, committing, and pushing them to the origin, follow these steps:

## Step-by-Step Instructions:

### 1. Navigate to the repository directory:

First, go to the games repository located at /usr/src/kodekloudrepos/games.

```bash
cd /usr/src/kodekloudrepos/games
```

### 2. List the Stashes
To check if there are any stashes and their respective identifiers, use the git stash list command
```bash
git stash list
```
This will display the stashes stored in your repository. Look for the stash with the identifier stash@{1}, which is what you need to restore.

### 3. Show the details of the stash (optional)
If you'd like to view changes are in `stash@{1}`, you can use the following command to show the diff.
```bash
git stash show -p stash@{1}
```
### 4.  Apply the stashed changes
To restore the stashed changes with the identifier stash@{1}, use the git stash apply command.
```bash
git stash apply stash @{1}
```
- This will restore the changes from the stash, but it does not remove ir from the stash list. if you want to remove from the stash list after applying it, use git stash drop command
### 5. Commit changes
Once the changes are restrored, you can commit them. First, check the status of the repo to see the changes
```bash
git status
```
Stage the changes:
```
git add .
```
Commit the changes with a meaningful commit message:
```
git commit -m "Restored and applied changes from stash@{1}"
```
### 6. Push the changes to the origin:
After committing the changes, push them to the remote repository (origin):
```
git push origin main
```
### Screenshots:

<img width="700" height="400" alt="image" src="https://github.com/user-attachments/assets/778ffbb9-6443-417f-99a0-40731024683e" />



