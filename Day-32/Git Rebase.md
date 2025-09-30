## Day-32: Git Rebase

Problem Statement:
The Nautilus application development team has been working on a project repository /opt/news.git. This repo is cloned at /usr/src/kodekloudrepos on storage server in Stratos DC. They recently shared the following requirements with DevOps team:

One of the developers is working on feature branch and their work is still in progress, however there are some changes which have been pushed into the master branch, the developer now wants to rebase the feature branch with the master branch without loosing any data from the feature branch, also they don't want to add any merge commit by simply merging the master branch into the feature branch. Accomplish this task as per requirements mentioned.

Also remember to push your changes once done.

## Solution:

To address the task of rebasing the feature branch onto the master branch without losing any changes from the feature branch and without creating a merge commit, you need to follow these steps:

## Steps to Rebase Feature Branch onto Master

### 1. **Navigate to the Repository**
First, navigate to the repository on your server.


```bash
cd /usr/src/kodekloudrepos/news
```

### 2. Check Out the Feature Branch

Make sure you're on the feature branch. If not, switch to it:

```bash
git checkout feature
```
### 3. Fetch the Latest Changes from the Remote Repository

Ensure your local repository is up to date by fetching the latest changes from the remote:
```
git fetch origin
```
### 4. Rebase Feature Branch onto Master

Rebase your local feature branch onto the latest master branch. This will apply your feature commits on top of the latest master commits.
```bash
git rebase origin/master
```
If there are any conflicts, Git will pause and notify you. You can resolve conflicts in the files marked by Git.

### 5. Resolve Conflicts (If Any)

If conflicts occur, Git will stop and prompt you to resolve them. To view the conflicting files, use:
```bash
git status
```
Resolve the conflicts in the files, then stage the changes:
```bash
git add <resolved-file>
```
Once all conflicts are resolved, continue the rebase:
```bash
git rebase --continue
```
Repeat this process until the rebase is completed.

### 6. Force Push the Feature Branch to the Remote Repository

Since a rebase rewrites commit history, you'll need to force-push the changes to the remote feature branch:
```bash
git push --force-with-lease origin feature
```
The --force-with-lease flag ensures that you don’t overwrite someone else's changes on the remote repository.

### 7. Verify the Commit History

You can check the log to verify that the rebase was successful and that your feature branch is based on the latest master commit:
```bash
git log --oneline --graph --all
```
This should display a history like:
```
* d6a7a9d (HEAD -> feature, origin/feature) Add new feature
* 1720a24 (origin/master, master) Update info.txt
* fbc178e initial commit
```
### 8. Push and Sync Changes (Optional)

If you haven’t already pushed the changes, make sure the remote repository is updated:
```bash
git push --force-with-lease origin feature
```

## Screenshots
<img width="1050" height="519" alt="image" src="https://github.com/user-attachments/assets/25baf4ef-e988-44cb-9f69-157d2cd9d2fa" />




