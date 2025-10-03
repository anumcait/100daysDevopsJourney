# Day 34: Git Hook ::: Automatically Create and Push Release Tags

## Problem Statement:

The Nautilus application development team was working on a git repository /opt/news.git which is cloned under /usr/src/kodekloudrepos directory present on Storage server in Stratos DC. 

The team want to setup a hook on this repository, please find below more details: Merge the feature branch into the master branch, but before pushing your changes complete below point. 

Create a post-update hook in this git repository so that whenever any changes are pushed to the master branch, it creates a release tag with name release-2023-06-15, where 2023-06-15 is supposed to be the current date. 

For example if today is 20th June, 2023 then the release tag must be release-2023-06-20. Make sure you test the hook at least once and create a release tag for today's release. 

Finally remember to push your changes. Note: Perform this task using the natasha user, and ensure the repository or existing directory permissions are not altered.

### Solution :
## What are Git Hooks

`Git hooks` are custom scripts that run at certain points in the Git lifecycle. You can use them to automate tasks like code linting, running tests, or validating commit messages before changes are actually made.

### Git Hook Basics:

Git hooks are located in the .git/hooks/ directory of your repository. There are several types of hooks, including:

Pre-commit Hook: Runs before a commit is finalized. You can use it to lint code or run tests.

Commit-msg Hook: Runs after a commit message is written but before the commit is completed. Useful for validating commit messages (e.g., enforcing a conventional commit format).

Pre-push Hook: Runs before code is pushed to a remote repository. You can use it to run integration tests or check if your branch is up to date.

Post-commit Hook: Runs after the commit is made, often used for notifications or logging purposes.

Objective

Set up a post-update Git hook in the /opt/news.git repository so that whenever changes are pushed to the master branch, a release tag is created with the format release-YYYY-MM-DD (based on the current date). For example, if today is October 3rd, 2025, the tag will be release-2025-10-03.

### Prerequisites

- The repository is located at /opt/news.git and is cloned in the /usr/src/kodekloudrepos/news directory.
- You are logged in as the natasha user.
- You have necessary permissions to modify the Git hooks.

### 1. Navigate to the Git Hooks Directory

Log in as natasha and navigate to the .git/hooks directory of your repository:
```bash
cd /opt/news.git/hooks
```
### 2. Create the post-update Hook

Create or edit the post-update file in the .git/hooks directory:
```bash
nano post-update
```
3. Add the Script to the Hook
4. In the post-update file, add the following script:
```
#!/bin/bash

# Get the current date in the format YYYY-MM-DD
CURRENT_DATE=$(date +'%Y-%m-%d')

# Tag name based on the current date
TAG_NAME="release-${CURRENT_DATE}"

# Check if the pushed branch is the master branch
if git rev-parse --abbrev-ref HEAD | grep -q 'master'; then
    # Create the release tag
    git tag "$TAG_NAME"

    # Push the tag to the remote repository
    git push origin "$TAG_NAME"

    # Optionally, print a success message
    echo "Release tag $TAG_NAME created and pushed successfully!"
fi
```
### 4. Make the Script Executable

Ensure that the post-update hook is executable:
```bash
chmod +x post-update
```
### 5. Test the Hook
#### 5.1 Merge Feature Branch into master

First, check if the feature branch exists locally or remotely:
```bash
git branch -r
```

If the feature branch exists remotely (e.g., origin/feature), fetch it and check it out:
```bash
git fetch origin
git checkout -b feature origin/feature
```

Now, switch to the master branch and merge the feature branch into master:
```bash
git checkout master
git merge feature
```

#### 5.2 Push Changes to Remote

Push the master branch to the remote:
```bash
git push origin master
```
This will trigger the post-update hook, which will create and push the release tag.
## 6. Verify the Release Tag
#### 6.1 Check for Local Tags

After pushing, check if the tag has been created locally by running:
```bash
git tag -l
```
#### 6.2 Fetch Remote Tags

If the tag doesn’t show up locally, fetch remote tags to update your local repository:
```bash
git fetch --tags
```
#### 6.3 Verify Remote Tags

You can also verify that the tag exists on the remote repository by running:
```bash
git ls-remote --tags origin
```

If the tag was pushed correctly, you should see it listed.

## 7. Manually Create and Push a Tag (Optional)

If the tag was not created automatically, you can manually create it and push it:

#### 7.1 Create the Tag
git tag release-$(date +'%Y-%m-%d')

#### 7.2 Push the Tag to the Remote
git push origin --tags

## 8. Verify the Tag's Commit

To inspect the commit that the tag points to, run:
```bash
git show release-$(date +'%Y-%m-%d')
```

This will show the commit associated with the tag.

### Troubleshooting

Tag Not Created Locally:
If the tag isn't showing locally, run git fetch --tags to update your local tags.

Error with Remote Access:
If there’s a fatal: Could not read from remote repository error, check your remote repository configuration:
```bash
git remote -v
```

Ensure the remote URL is correct and you have access rights.
