# Day 24: Git Create Branches kodekloud 100 days challenge 

**Problem Statement**

Nautilus developers are actively working on one of the project repositories, /usr/src/kodekloudrepos/news. 

Recently, they decided to implement some new features in the application, and they want to maintain those new changes in a separate branch. 

Below are the requirements that have been shared with the DevOps team: On Storage server in Stratos DC create a new branch xfusioncorp_news from master branch in /usr/src/kodekloudrepos/news git repo. 

Please do not try to make any changes in the code.

**Solution**

To fulfill the request and create a new branch named xfusioncorp_news from the master branch in the repository /usr/src/kodekloudrepos/news on the storage server in the Stratos DC, follow these steps:

### Steps:

#### 1. Login to the server:

```bash
ssh natasha@ststor01
```
#### 2. Navigate to the repository:
- Go to the directory where the news repository is located.
```bash
cd /usr/src/kodekloudrepos/news
```
#### 3. Check the current status:
- Before creating the new branch, ensure that you’re on the master branch and that everything is up-to-date.
```bash
sudo git status
```
- If you're not on the master branch, switch to it:
```bash
sudo git checkout master
```
- Pull the latest changes to ensure your `master` branch is up-to-date:
```bash
sudo git pull origin master
```
#### 4. Create the new branch xfusioncorp_news:
- Create and switch to the new branch:
```bash
sudo git checkout -b xfusioncorp_news
```
#### 5. Push the new branch to the remote repository:
Push the newly created branch to the remote repository:
```bash
sudo git push origin xfusioncorp_news
```
#### 6. Verify:
- You can verify that the new branch was created and pushed successfully by listing all branches:
```bash
sudo git branch -a
```

You should see the xfusioncorp_news branch listed.

<img width="907" height="444" alt="image" src="https://github.com/user-attachments/assets/97e7e492-eab8-4e74-ba1e-be023b0146f4" />


