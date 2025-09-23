## Day 25: Git Merge Branches kodekloud 100days challenge

### Problem Statement

The Nautilus application development team has been working on a project repository /opt/games.git. This repo is cloned at /usr/src/kodekloudrepos on storage server in Stratos DC. 

They recently shared the following requirements with DevOps team: Create a new branch datacenter in /usr/src/kodekloudrepos/games repo from master and copy the /tmp/index.html file (present on storage server itself) into the repo. 

Further, add/commit this file in the new branch and merge back that branch into master branch. 

Finally, push the changes to the origin for both of the branches.


# Git Merge Branches: A Step-by-Step Guide

This guide walks through the process of creating a new Git branch, adding a file, committing changes, and merging the branch back into `master`. The task is part of the **Nautilus Application Development Team** project repository `/opt/games.git`, cloned at `/usr/src/kodekloudrepos` on the Stratos DC storage server.

## Task Requirements
- Create a new branch `datacenter` from the `master` branch.
- Copy the `/tmp/index.html` file (which exists on the server) into the repository.
- Add and commit this file in the new branch.
- Merge the `datacenter` branch back into the `master` branch.
- Push both branches (`datacenter` and `master`) to the remote origin.

---

## Prerequisites

- **Git** should be installed on the system.
- You should have access to the **storage server** where the repo is cloned (`/usr/src/kodekloudrepos/games`).
- Ensure you have the **correct permissions** to modify the repository and push changes to the remote.

## Steps to Complete the Task

### 1. Navigate to the Repository

Start by navigating to the correct directory where the repository is cloned.

```bash
cd /usr/src/kodekloudrepos/games
```
### 2. Check the Current Branch

Make sure you're on the master branch before creating a new branch.

```bash
git checkout master
```

### 3. Create a New Branch datacenter

Create a new branch from master called datacenter and switch to it.
```bash
git checkout -b datacenter

```
### 4. Copy the index.html File into the Repository

Copy the index.html file from the /tmp directory into the current directory of the Git repository.

```bash
cp /tmp/index.html .
```
### 5. Add the File to Git Staging

Add the newly copied index.html file to the staging area in Git.
```bash
git add index.html
```
### 6. Commit the Changes

Commit the added file with a message describing what was done.

```bash
git commit -m "Add index.html to datacenter branch"
```
### 7. Switch Back to the master Branch

Once the changes are committed, switch back to the master branch to merge the datacenter branch into it.
```bash
git checkout master
```
### 8. Merge the datacenter Branch into master

Merge the changes from datacenter into the master branch.
```bash
git merge datacenter
```

### 9. Push the Changes to the Remote Repository

Push both the datacenter and master branches to the remote repository to ensure that all changes are shared with the team.
```bash
git push origin datacenter
git push origin master
```
## Summary of Commands
cd /usr/src/kodekloudrepos/games
git checkout master
git checkout -b datacenter
cp /tmp/index.html .
git add index.html
git commit -m "Add index.html to datacenter branch"
git checkout master
git merge datacenter
git push origin datacenter
git push origin master

----
## Screenshots
<img width="600" height="400" alt="image" src="https://github.com/user-attachments/assets/a5c8e6af-3b17-45a2-a448-6100c289f331" />
<img width="600" height="400" alt="image" src="https://github.com/user-attachments/assets/18f535f6-da62-4a24-8b03-90b6e30e0835" />

