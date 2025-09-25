#🚀 Day 27: Git Revert Some Changes 🚀


## Problem statement

The Nautilus application development team was working on a git repository /usr/src/kodekloudrepos/games present on Storage server in Stratos DC. 

However, they reported an issue with the recent commits being pushed to this repo. They have asked the DevOps team to revert repo HEAD to last commit. Below are more details about the task:

In /usr/src/kodekloudrepos/games git repository, revert the latest commit ( HEAD ) to the previous commit (JFYI the previous commit hash should be with initial commit message ).

Use revert apps message (please use all small letters for commit message) for the new revert commit.

## Solution

To revert the latest commit (HEAD) to the previous commit and use the specified commit message, you can follow these steps:

### Steps to revert the latest commit:

**Navigate to the repository:**
Go to the directory where the git repository is located.

cd /usr/src/kodekloudrepos/media


**Check the commit history:**
This will help you verify the recent commits and confirm the HEAD is pointing to the commit you want to revert.

```
git log --oneline
```

This will give you a list of commits in the repository. The most recent commit will be the HEAD commit.

Revert the latest commit:
Use the git revert command to undo the latest commit. This will create a new commit that undoes the changes made in the last commit.

```
git revert HEAD
```

Edit the commit message:
By default, the revert will open the default text editor with a pre-filled message. You need to edit this message to meet the requirement of "revert media" (all lowercase).

For example:

revert media


After editing, save and close the editor.

Verify the commit:
You can check the log to ensure the new revert commit has been created.

```
git log --oneline
```

Push the changes:
Once you're satisfied with the revert, push the changes back to the remote repository.

```
git push origin main
```

### Recap of commands:
cd /usr/src/kodekloudrepos/media
git log --oneline
git revert HEAD
# Edit commit message to "revert media" (replace first line with this)
git push origin main


That should successfully revert the latest commit with the specified message and push it to the remote repository.

### Screenshots

<img width="1050" height="519" alt="image" src="https://github.com/user-attachments/assets/fb5aca49-08ba-48ad-9c88-ef821e23fcb8" />



