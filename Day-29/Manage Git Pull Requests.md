# 🚀 Day 29: Manage Git Pull Requests – KodeKloud 100 Days Challenge

Problem Statement : Max want to push some new changes to one of the repositories but we don't want people to push directly to master branch, since that would be the final version of the code. 

It should always only have content that has been reviewed and approved. We cannot just allow everyone to directly push to the master branch. So, let's do it the right way as discussed below: 

SSH into storage server using user max, password Max_pass123 . 

There you can find an already cloned repo under Max user's home. Max has written his story about The 🦊 Fox and Grapes 🍇 Max has already pushed his story to remote git repository hosted on 

Gitea branch story/fox-and-grapes Check the contents of the cloned repository. Confirm that you can see Sarah's story and history of commits by running git log and validate author info, commit message etc. 

Max has pushed his story, but his story is still not in the master branch. Let's create a Pull Request(PR) to merge Max's story/fox-and-grapes branch into the master branch Click on the Gitea UI button on the top bar. 

You should be able to access the Gitea page. UI login info: - Username: max - Password: Max_pass123 PR title : Added fox-and-grapes story PR pull from branch: 

story/fox-and-grapes (source) PR merge into branch: master (destination) Before we can add our story to the master branch, it has to be reviewed. 

So, let's ask tom to review our PR by assigning him as a reviewer Add tom as reviewer through the Git Portal UI Go to the newly created PR Click on Reviewers on the right Add tom as a 

reviewer to the PR Now let's review and approve the PR as user Tom Login to the portal with the user tom Logout of Git Portal UI if logged in as max UI login info: - Username: tom - Password: Tom_pass123 PR title : Added fox-and-grapes story Review and merge it. Great stuff!! The story has been merged! 👏 Note: For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.

## 📝 Objective:
Prevent direct commits to the `master` branch by using feature branches and pull requests. Max will push a new story and create a pull request to merge it into `master` after review.

---

## 👤 User Credentials

- **Max**
  - Username: `max`
  - Password: `Max_pass123`

- **Tom**
  - Username: `tom`
  - Password: `Tom_pass123`

---

## ✅ Step-by-Step Instructions

### 1. SSH into the Storage Server as Max

```bash
ssh max@<storage_server_ip>
# password: Max_pass123
```
2. Navigate to the Git Repository
cd ~
ls -la
cd <repo-folder-name>

### 3. Verify Git History
```bash
git log --oneline --graph --decorate --all
```

✅ Ensure you see:

- Commits by Sarah
- Max's new branch: story/fox-and-grapes

### 4. Check the Branch
```bash
git branch -a
git checkout story/fox-and-grapes
cat fox-and-grapes.md
```
### 5. Confirm Branch is Pushed to Remote
```bash
git push origin story/fox-and-grapes
```
### 6. Open Gitea Web UI

Click the "Gitea" button on the KodeKloud top bar.

### 7. Login to Gitea as Max

- Username: max
- Password: Max_pass123

8. Create a Pull Request (PR)

  1. Navigate to the repository
  2. Go to Pull Requests → New Pull Request
  3. Set the following:
     - From branch: story/fox-and-grapes
     - To branch: master
  4. Set PR Title: Added fox-and-grapes story
  5. Click **Create Pull Request**

### 9. Add Reviewer (Tom)

- On the PR page, click Reviewers (right-hand side)
- Select or type tom and assign as a reviewer

### 10. Logout and Login as Tom
Logout of the UI, then login as:
- Username: tom
- Password: Tom_pass123

### 11. Review and Merge PR

1. Go to the Pull Requests section
2. Open the PR titled Added fox-and-grapes story
3. Click Review Changes → Approve
4. Click Merge Pull Request
5. Confirm the merge

### 🧾 Final Confirmation

Switch to the master branch in Gitea UI and confirm:
  - Max’s story file (e.g., fox-and-grapes.md) is now part of the master branch

### Screenshots

<img width="1050" height="534" alt="image" src="https://github.com/user-attachments/assets/1805f972-5e2a-4aff-a29c-5a8ec8fe8544" />

<img width="1050" height="467" alt="image" src="https://github.com/user-attachments/assets/5fbaf7f9-69f3-40f3-a27c-e6cd4a037b3b" />

<img width="1050" height="422" alt="image" src="https://github.com/user-attachments/assets/18a505d2-07a4-495d-9433-02542229f1e3" />




