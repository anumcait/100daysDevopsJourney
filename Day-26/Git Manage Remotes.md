# Day 26: Git Manage Remotes

### Problem Statement

The xFusionCorp development team added updates to the project that is maintained under /opt/blog.git repo and cloned under /usr/src/kodekloudrepos/blog. 

Recently some changes were made on Git server that is hosted on Storage server in Stratos DC. The DevOps team added some new Git remotes, so we need to update remote on /usr/src/kodekloudrepos/blog repository as per details mentioned below: 

a. In /usr/src/kodekloudrepos/blog repo add a new remote dev_blog and point it to /opt/xfusioncorp_blog.git repository. 

b. There is a file /tmp/index.html on same server; copy this file to the repo and add/commit to master branch. 

c. Finally push master branch to this new remote origin.

## Task Overview

The xFusionCorp development team recently updated the project maintained under `/opt/blog.git` and cloned to `/usr/src/kodekloudrepos/blog`. The DevOps team added new Git remotes on the Git server hosted on the Storage server in Stratos DC. We need to update the remotes for the `/usr/src/kodekloudrepos/blog` repository as per the following steps:

1. Add a new remote `dev_blog` pointing to `/opt/xfusioncorp_blog.git`.
2. Copy the file `/tmp/index.html` to the repository, and commit it to the `master` branch.
3. Push the changes to the new remote `dev_blog` for the `master` branch.

---

## Steps to Complete the Task

### Step 1: Add a New Remote

To add a new remote called `dev_blog` pointing to `/opt/xfusioncorp_blog.git`, use the following command:

```bash
cd /usr/src/kodekloudrepos/blog
git remote add dev_blog /opt/xfusioncorp_blog.git
```
This command adds the remote repository and names it dev_blog.

### Step 2: Copy the index.html File to the Repository

Copy the file /tmp/index.html to your local repository directory:
```bash
cp /tmp/index.html /usr/src/kodekloudrepos/blog
```

Now, add and commit the new file to the repository:

```BASH
git add index.html
git commit -m "Add index.html file"
```

This will stage the index.html file and commit it with a message.

### Step 3: Push the Changes to the New Remote

Finally, push the changes to the master branch of the newly added remote dev_blog:
```bash
git push dev_blog master
```

This will push the master branch to the dev_blog remote.

### Summary of Commands
cd /usr/src/kodekloudrepos/blog
git remote add dev_blog /opt/xfusioncorp_blog.git
cp /tmp/index.html /usr/src/kodekloudrepos/blog
git add index.html
git commit -m "Add index.html file"
git push dev_blog master

### Conclusion

By following these steps, you have added a new remote, committed changes to the repository, and successfully pushed the master branch to the new remote dev_blog.

### Screenshots

<img width="700" height="524" alt="image" src="https://github.com/user-attachments/assets/d70fa6fa-a691-40c9-9d0f-15589e774593" />

<img width="700" height="546" alt="image" src="https://github.com/user-attachments/assets/2b68ad56-c075-4009-b2b9-26a758fb1f4f" />




