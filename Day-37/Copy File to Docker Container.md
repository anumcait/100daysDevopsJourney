# Day 37: Copy File to Docker Container

**Problem Statement:**
The Nautilus DevOps team possesses confidential data on App Server 1 in the Stratos Datacenter. A container named ubuntu_latest is running on the same server.

Copy an encrypted file /tmp/nautilus.txt.gpg from the docker host to the ubuntu_latest container located at /home/. Ensure the file is not modified during this operation.

---

## Solution:

### ✅ Task Summary:

- Docker container name: ubuntu_latest
- Source file on host: /tmp/nautilus.txt.gpg
- Target path inside container: /home/
- Ensure no modification of the file (copy as-is)

### 🛠️ Steps to Solve

**1. Make sure the container is running**
```bash
docker ps
```
You should see ubuntu_latest in the list.

**2. Use docker cp to copy the file from host to container**
This command is used to copy files between the host and a container.

Run:
```bash
docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/home/
```
This will copy the file as-is, ensuring no modification.

### 3. (Optional) Verify inside the container
You can check if the file was copied successfully:
```bash
docker exec -it ubuntu_latest bash
ls -l /home/nautilus.txt.gpg
```

## Screenshots
<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/45ec50db-94a3-4e44-bbb1-ed1c16459602" />

<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/ee7c6020-b27c-4aba-9966-90f594f3d996" />




