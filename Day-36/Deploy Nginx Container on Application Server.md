# Day 36 - Deploy Nginx Container on Application Server

**Problem Statement:**
The Nautilus DevOps team is conducting application deployment tests on selected application servers. They require a nginx container deployment on Application Server 1. Complete the task with the following instructions: 

On Application Server 1 create a container named nginx_1 using the nginx image with the alpine tag. Ensure container is in a running state.

## ✅ Task Summary

- **Container Name**: `nginx_1`
- **Image**: `nginx:alpine`
- **Server**: Application Server 1
- **Status**: Container should be in a **running** state

---

## 🧰 Prerequisites

- Ensure you have access to **Application Server 1**
- Ensure Docker is installed and running

---

## 🚀 Step-by-Step Instructions

### 1. Connect to Application Server 1

If using a jump host:

```bash
ssh tony@stapp01  # Replace with actual server/user if different
```
### 2. Verify Docker is Installed
docker --version
If Docker is not installed, install it using the commands below:

<details> <summary>📦 For CentOS/RHEL</summary>
  
sudo yum install docker -y
  
sudo systemctl start docker

sudo systemctl enable docker

</details> 

<details> <summary>📦 For Ubuntu/Debian</summary>
  
sudo apt update
  
sudo apt install docker.io -y

sudo systemctl start docker

sudo systemctl enable docker

</details>

### 3. Pull the Nginx Alpine Image

docker pull nginx:alpine

### 4. Run the Nginx Container

docker run -d --name nginx_1 nginx:alpine

Explanation:

- `-d`: Run container in detached mode
- `--name nginx_1`: Set container name
- `nginx:alpine`: Use Alpine-based Nginx image

### 5. Verify the Container is Running
```bash
docker ps
```
### Expected output:
```mathematica
CONTAINER ID   IMAGE          COMMAND                  STATUS         NAMES
abcd1234       nginx:alpine   "/docker-entrypoint.…"   Up X seconds   nginx_1
```
**You have successfully deployed the Nginx container using the nginx:alpine image on Application Server 1.**

## Screenshots:
<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/cfdbbcdd-7478-45e5-b77d-93130236a9c5" />

