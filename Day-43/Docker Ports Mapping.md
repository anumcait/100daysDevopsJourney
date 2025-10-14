# Day-43 - Docker Nginx Port Mapping Setup

## Overview
This project demonstrates how to set up a Docker container running **Nginx** and map a host port to the container's port using Docker commands. 

This setup allows access to the Nginx web server running inside the container on a specific host port.

This task was completed as part of the **Kodekloud 100 Days of DevOps Challenge** on **Day 43**.

## Steps to Recreate

### 1. Pull the Nginx Image
First, pull the official **nginx:alpine** Docker image:

```bash
docker pull nginx:alpine
```
### 2. Create a Docker Container

Next, create and run the container named official using the pulled Nginx image. Use the -d flag to run it in detached mode (in the background):

```bash
docker run --name official -d nginx:alpine
```
### 3. Map Host Port 8083 to Container Port 80

To expose the Nginx service to the outside world, map port 8083 on the host machine to port 80 inside the container:

```bash
docker run --name official -d -p 8083:80 nginx:alpine
```
- -p 8083:80 tells Docker to map port 8083 on the host to port 80 on the container, allowing you to access the Nginx service via http://<host-ip>:8083.

### 4. Verify the Container is Running

You can verify that the container is running and the port mapping is correct by using the following command:
```bash
docker ps
```
This will list all the running containers along with their port mappings. Look for the 8083->80 mapping to ensure everything is set up correctly.

### 5. Access the Nginx Server

Open a web browser and go to http://<host-ip>:8083. You should see the default Nginx welcome page, confirming that the container is running and accessible.

### Key Takeaways

**`Port Mapping in Docker:`** By using the -p flag, you can map specific host ports to container ports, making it easy to access containerized applications externally.

**`Networking with Containers:`** Understanding how port mappings work is crucial when you want your containers to interact with the outside world (or other containers).

### Screenshots:

<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/d2f43d8c-ef5e-45d9-b088-dee024052d29" />

<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/114e8556-23e5-4571-9a52-b1f860a5ebde" />



