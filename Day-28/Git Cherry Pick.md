## Day 28: Git Cherry Pick 

### Problem Statement
The Nautilus application development team has been working on a project repository /opt/blog.git. 

This repo is cloned at /usr/src/kodekloudrepos on storage server in Stratos DC. 

They recently shared the following requirements with the DevOps team: 
There are two branches in this repository, master and feature. One of the developers is working on the feature branch and their work is still in progress, 
however they want to merge one of the commits from the feature branch to the master branch, the message for the commit that needs to be merged into master is Update info.txt. 

Accomplish this task for them, also remember to push your changes eventually.
## Solution

### Cherry-Pick Commit from `feature` to `master` Branch

The task was to cherry-pick the commit `Update info.txt` from the `feature` branch into the `master` branch of the repository. Below are the detailed steps to accomplish this:

### Steps to Cherry-Pick a Commit

### 1. **Navigate to the Repository**
Ensure you are in the correct repository directory on your server.

```bash
cd /usr/src/kodekloudrepos
```
### 2. Check Out the master Branch

Switch to the master branch to prepare for the cherry-pick.
```bash
sudo git checkout master
```
### 3. Find the Commit to Cherry-Pick

- Switch to the feature branch to locate the commit you want to cherry-pick.
```bash
sudo git checkout feature
```

- Run the git log command to view the commit history and find the hash of the commit labeled "Update info.txt".
```bash
sudo git log --oneline
```

This will output something like:
```bash
175a363 (HEAD -> feature, origin/feature) Update welcome.txt
b8dec39 Update info.txt
98710c0 (origin/master, master) Add welcome.txt
50a6ec7 initial commit
```

- Here, b8dec39 is the commit you need to cherry-pick.

### 4. Cherry-Pick the Commit

- Switch back to the master branch:
```bash
sudo git checkout master
```

- Now, use git cherry-pick to apply the commit b8dec39 (commit hash for Update info.txt) from the feature branch to master.
- sudo git cherry-pick b8dec39

### 5. Resolve Conflicts (If Any)

If there are any conflicts during the cherry-pick, Git will prompt you to resolve them. After resolving the conflicts, add the files to stage them and continue the cherry-pick process.
```bash
sudo git add <resolved-files>
sudo git cherry-pick --continue
```
### 6. Push Changes to the Remote Repository

Once the cherry-pick is complete, push the changes to the remote master branch.
```bash
sudo git push origin master
```
## Verifying the Cherry-Pick

### After performing the cherry-pick, you can verify that everything was applied correctly:

- 1. Check the Commit Log

Run git log to confirm that the cherry-pick is visible in the master branch.
```bash
sudo git log --oneline
```

- You should see a commit with the message "Update info.txt":
```bash
a3ec1cc (HEAD -> master, origin/master) Update info.txt
98710c0 Add welcome.txt
50a6ec7 initial commit
```
- 2. Verify the File Content

Check the contents of the info.txt file to make sure the changes are reflected.

cat info.txt

- 3. Compare with the Feature Branch

To double-check that the cherry-picked commit is the same, you can compare the contents of info.txt between feature and master:

sudo git diff master..feature -- info.txt


This will show you any differences between the master and feature branch, specifically for info.txt.

- 4. Verify the Remote Repository

Finally, ensure the remote master branch has been updated by fetching the latest changes from the remote:

sudo git fetch origin
sudo git log origin/master --oneline


This should show the same commit a3ec1cc with the message "Update info.txt" in the remote repository.

## Summary

- We successfully cherry-picked the "Update info.txt" commit from the feature branch to the master branch.
- The commit is visible in the local log and remote repository.
- The file info.txt has been updated as intended.
- The changes have been pushed to the remote master branch.

## Screenshots:
<img width="700" height="400" alt="image" src="https://github.com/user-attachments/assets/94e85c64-6cb3-4f1f-b9e8-1eecc4b44533" />

