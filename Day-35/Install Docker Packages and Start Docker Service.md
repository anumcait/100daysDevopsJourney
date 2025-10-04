# ✅ Day 35 : Install Docker Packages and Start Docker Service

Problem Statement: The Nautilus DevOps team aims to containerize various applications following a recent meeting with the application development team. They intend to conduct testing with the following steps: Install docker-ce and docker compose packages on App Server 1. Initiate the docker service.

## 📘 What is Docker - A Quick Explaination before going into installation

**Docker is a containerization platform that allows you to package applications with all their dependencies into isolated containers. These containers can run consistently across any system, making deployment faster and more reliable.
**
- Lightweight compared to Virtual Machines and portable in nature
- Mainly solves the ancient problem like works in my system but not in other system.
- Fast startup and resource-efficient
- Ideal for DevOps, testing, and microservices

## 🗓️ Task: Install Docker Packages and Start Docker Service

### 🔧 Objective

- Install `docker-ce` and Docker Compose plugin on **App Server 1**.
- Start and enable the Docker service.
- Verify Docker is running successfully.

---

## 📦 Steps Performed

### 1. SSH into App Server 1

```bash
ssh tony@stapp01
```
🔸 Ensure you're working on App Server 1, not the jump host.
---
## 2. Install Docker and Compose Plugin
```bash
sudo dnf -y install dnf-plugins-core
sudo dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo dnf install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

```
---
## 3. Start and Enable Docker
```bash
sudo systemctl enable docker
sudo systemctl start docker
(or)
sudo systemctl enable --now docker

```
(This ensures Docker starts now and on every reboot.
---
## 4. Verify Installation
```bash
docker --version
docker compose version
sudo docker run hello-world
```

## ✅ Status

- Docker installed successfully
- Docker service is active and enabled
- Verified with hello-world container

## Screenshots
<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/d89b04aa-2f36-4ae0-b29d-1b42501a00e5" />
<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/a1d4de97-d903-4025-a014-b51ff8a1ef7d" />



