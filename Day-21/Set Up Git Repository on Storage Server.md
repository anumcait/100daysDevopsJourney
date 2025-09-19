## 🚀 Day 21: Set Up Git Repository on Storage Server

**Problem Statement**

kodekloud 21 day challenge The Nautilus development team has provided requirements to the DevOps team for a new application development project, specifically requesting the establishment of a Git repository. Follow the instructions below to create the Git repository on the Storage server in the Stratos DC: Utilize yum to install the git package on the Storage Server. Create a bare repository named /opt/beta.git (ensure exact name usage).

## ✅ Task Summary

Set up a Git repository on the Storage Server for the Nautilus development team as per the provided requirements.

---

### 1. Installed Git using `yum`:
```bash
sudo yum install git -y
```
### 2. Created a bare Git repository:
sudo git init --bare /opt/beta.git

**📁 Why a Bare Repository?**

A bare Git repository contains only the .git version control data — it has no working directory. It’s perfect for remote collaboration, especially in centralized workflows where developers push/pull from a central repository.

**📌 Repository Path**
/opt/beta.git

**🧠 Concepts Practiced**

- Git installation on RHEL/CentOS
- Bare vs normal Git repositories
- Working with Linux file system paths
- Server-side Git configuration

### 📸 Screenshot

<img width="759" height="375" alt="image" src="https://github.com/user-attachments/assets/adf29aeb-6912-4a12-bf20-68137dda0234" />

