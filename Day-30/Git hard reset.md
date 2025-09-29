# Day-30 : Git Hard Reset 

### Problem Statement :

The Nautilus application development team was working on a git repository /usr/src/kodekloudrepos/official present on Storage server in Stratos DC. 


This was just a test repository and one of the developers just pushed a couple of changes for testing, but now they want to clean this repository along with the commit history/work tree, so they want to point back the HEAD and the branch itself to a commit with message add data.txt file. Find below more details: In /usr/src/kodekloudrepos/official git repository, reset the git commit history so that there are only two commits in the commit history i.e initial commit and add data.txt file. 

Also make sure to push your changes.

### Solution:

1. To complete this task, we need to login to the storage server with

```bash
ssh natasha@ststor01
```
it asks for confirmation, say 'Yes', and again asks for Password, give password and now you are into storage server.

2. Go to the location where it is mentioned exactly

```bash
cd /usr/src/kodekloudrepos/official
```
3. Then listout the git history using

```bash
sudo git log --oneline

Output in my case:
e37fa1a (HEAD -> master, origin/master) Test Commit10
199fa18 Test Commit9
b87585a Test Commit8
87a1d76 Test Commit7
6da330a Test Commit6
c1f2e28 Test Commit5
c270024 Test Commit4
e4ca750 Test Commit3
8583378 Test Commit2
2f63b1f Test Commit1
22b831a add data.txt file
8532922 initial commit
```
4. We found the add data.txt file at 22b831a , so we want to reset the repository so that there are only two commints inital commit and the commit with the message `add data.txt file`

5. Reset the commit history

First, reset the history to the commit message `add data.txt file (22b831a)`. this will leave only one `initial commit` and the `add data.txt file` commit in the history.
```bash
sudo git reset --hard 22b831a
```
6. Force push changes to the repository
After performing the reset, force-push the changes to the remote repository to ensure history is rewritten.
```bash
sudo git push origin HEAD --force
```

### Verify the Changes

1. Check the commit history

```bash
sudo git log --oneline

```

the out put should show left history only.
```sql
22b831a add data.txt file
8532922 initial commit
```
2. Check the remote repository
After the push , you can verify the changes were successfully applied to the remote repository.
```bash
sudo git fetch
sudo log origin/master --oneline
```

### Screens
<img width="700" height="400" alt="image" src="https://github.com/user-attachments/assets/9908a5bd-d0ad-47bc-8d27-fa3c602d5f46" />







