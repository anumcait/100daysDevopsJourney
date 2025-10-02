# Day-33 Resolve Git Merge Conflicts

Problem Statement:

Sarah and Max were working on writting some stories which they have pushed to the repository. 

Max has recently added some new changes and is trying to push them to the repository but he is facing some issues. 

Below you can find more details: SSH into storage server using user max and password Max_pass123. Under /home/max you will find the story-blog repository. 

Try to push the changes to the origin repo and fix the issues. The story-index.txt must have titles for all 4 stories. Additionally, there is a typo in The Lion and the Mooose line where Mooose should be Mouse. Click on the Gitea UI button on the top bar. You should be able to access the Gitea page. You can login to Gitea server from UI using username sarah and password Sarah_pass123 or username max and password Max_pass123. Note: For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. 

You may also consider using a screen recording software such as loom.com to record and share your work.
# Resolving Git Merge Conflict and Pushing Changes

In this guide, we will walk through the process of resolving a Git merge conflict in the `story-index.txt` file and pushing the changes to the remote repository.

## Prerequisites
- You have access to the Git repository with max username with password Max_pass123.
- You are working on the `master` branch.
- You have SSH access or can use a terminal to interact with Git.
- You have already cloned the repository or have it locally on your machine.

## Steps to Resolve the Merge Conflict

### 1. **Pull the Latest Changes from the Remote Repository**

Start by pulling the latest changes from the remote `master` branch. This might result in a merge conflict if there are changes on the remote that you don’t have locally.

```bash
git pull origin master
```
If there are conflicts, Git will notify you of the merge conflicts and provide markers in the affected files.

### 2. Check for Conflicts
After the pull, Git will attempt to merge the changes but might fail due to conflicts. You’ll see an error message like this:

```bash
Auto-merging story-index.txt
CONFLICT (add/add): Merge conflict in story-index.txt
Automatic merge failed; fix conflicts and then commit the result.
```
This indicates that the story-index.txt file is in conflict. You need to manually resolve this conflict.

### 3. Open the Conflicted File (story-index.txt)
Open the story-index.txt file in your preferred text editor. The conflict will be marked like this:

```
<<<<<<< HEAD
Your local changes to story-index.txt.
=======
Changes from the remote repository to story-index.txt.
>>>>>>>
```
HEAD refers to your local changes.

The part after the ======= marker shows the changes from the remote repository.

### 4. Resolve the Conflict
Manually merge the changes by keeping the necessary lines and removing the conflict markers (<<<<<<<, =======, and >>>>>>>). You may want to preserve both sets of changes, or decide which one is more appropriate based on the context.

For example, after resolving, your story-index.txt file might look like this:

```
1. Story Title 1
2. Story Title 2
3. The Lion and the Mouse  # Fixed typo (Mooose -> Mouse)
4. Story Title 4
5. Stage the Resolved File
```
After resolving the conflict, save the file and add it to the staging area:

```bash
git add story-index.txt
```
### 6. Commit the Merge
Now that the conflict is resolved, commit the changes to complete the merge:

```bash
git commit -m "Resolved merge conflict in story-index.txt"
```
This commit will include the resolved changes from both the local and remote branches.

### 7. Push the Changes to the Remote Repository
Finally, push your local branch to the remote repository. Since you’ve resolved the conflict, the push should now succeed without errors:

```bash

git push origin master
```
### 8. Verify the Changes in the Remote Repository
To verify that everything went smoothly:

#### Go to the Gitea UI and log in.

Check the repository to ensure that your changes were pushed correctly.

Verify that the story-index.txt file contains the correct titles, and the typo ("Mooose" changed to "Mouse") is fixed.

Troubleshooting
If you encounter any issues during the process, consider the following:

1. If you can't push due to another conflict:
Make sure you've pulled the latest changes before pushing.

Ensure all conflicts are resolved, and all necessary files are staged (git add).

2. If you're unsure which changes to keep:
Always communicate with your team about how to merge conflicting changes if you're unsure.

Review both sets of changes carefully and decide which ones are appropriate.

