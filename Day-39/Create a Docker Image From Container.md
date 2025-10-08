# Day 39: Create a Docker Image From Container 

### Problem Statement:

One of the Nautilus developer was working to test new changes on a container. He wants to keep a backup of his changes to the container. 

A new request has been raised for the DevOps team to create a new image from this container. Below are more details about it:

a. Create an image cluster:nautilus on Application Server 3 from a container ubuntu_latest that is running on same server.

---

### Solution:

✅ Create a Docker image from a running container named ubuntu_latest on Application Server 3, and name the new image cluster:nautilus.

## ✅ Step-by-step Instructions
### 🔹 1. SSH into Application Server 3
If you're using KodeKloud's lab environment, connect to Application Server 3 using:

ssh tony@stapp03
Use the appropriate credentials if asked.

### 🔹 2. Confirm the container ubuntu_latest is running
Run:
```bash
docker ps
```
You should see a container named ubuntu_latest running. Example output:

CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    NAMES
a1b2c3d4e5f6   ubuntu    "/bin/bash"  ...     Up ...    ubuntu_latest

### 🔹 3. Commit the container to an image
Now create an image named cluster:nautilus from the running container:
```bash
docker commit ubuntu_latest cluster:nautilus
```
✅ This command creates a new image from the current state of the container.

### 🔹 4. Verify the image has been created
Run:
```bash
docker images
```
You should see:

REPOSITORY   TAG       IMAGE ID       CREATED          SIZE
cluster      nautilus  <some_id>      <a few seconds ago>   <size>


### Screenshots:

<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/468be3f8-18b7-4f39-a707-a9e60fd0cc2b" />
<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/97f2ab3c-d83e-4a87-9d70-5af5b89c6ff4" />

---



