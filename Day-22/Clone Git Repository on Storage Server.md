# Day 22: Clone Git Repository on Storage Server – KodeKloud 100 Days Challenge

### I found there is some issue while checking , but keep tying until you get the result

### Problem Satement
The DevOps team established a new Git repository last week, which remains unused at present. However, the Nautilus application development team now requires a copy of this repository on the Storage Server in the Stratos DC. Follow the provided details to clone the repository:

The repository to be cloned is located at /opt/apps.git

Clone this Git repository to the /usr/src/kodekloudrepos directory. Perform this task using the natasha user, and ensure that no modifications are made to the repository or existing directories, such as changing permissions or making unauthorized alterations.

## 📌 Task Summary

The DevOps team created a new Git repository last week (`/opt/apps.git`), and the application development team now requires a copy of this repository on the Storage Server in the **Stratos DC**.

I was tasked with cloning the repository properly using the following criteria:

- 🔐 User: `natasha` (non-root)
- 📂 Repository: `/opt/apps.git`
- 🧩 Target Directory: `/usr/src/kodekloudrepos/apps`
- ✅ Must not use `sudo`
- 🚫 Must not modify or delete any files or directories
- 🔁 Must use `git clone` (not `cp` or any manual copying)

---

## ✅ Steps I Followed

```bash
# 1. SSH into the storage server
Login as natasha:

ssh natasha@ststor01

# 2. Create the target directory (Ensure destination directory exists)
mkdir -p /usr/src/kodekloudrepos

# 3. Navigate to the target parent directory
cd /usr/src/kodekloudrepos

# 4. Clone the Git repository properly
git clone /opt/apps.git

# 5. Verify the clone
cd apps
ls -la
git log --oneline
```
### 🧪 Validation

- .git/ folder was present ✅
- Repository was not empty (commits existed) ✅
- No sudo used ❌
- Cloned to correct path: /usr/src/kodekloudrepos/apps ✅
- Task passed ✅

### 💡 Key Learnings

- Always clone from the correct user
- Avoid using sudo unless required
- Don’t clone inside the target directory — use its parent
- Bare repositories (.git) don’t show files unless they have commits
- Use git log or ls to verify cloning was successful

### Output Screenshot

<img width="1137" height="572" alt="image" src="https://github.com/user-attachments/assets/aa473fed-fa0d-4a42-a85b-f6113382eb60" />



