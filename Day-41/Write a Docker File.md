# Day 41: Write a Docker File

## Objective

As per the recent requirements from the Nautilus application development team, we are tasked with creating a custom Docker image using ubuntu:24.04 as the base image, installing Apache2, and configuring it to run on port 8083 (without modifying any other Apache configuration like document root).

---

## Steps

### Step 1: Create the Dockerfile

Create a file named Dockerfile (with a capital "D") in the /opt/docker/ directory of App Server 2.

Dockerfile Content

```Dockerfile
# Step 1: Use ubuntu:24.04 as the base image
FROM ubuntu:24.04

# Step 2: Install Apache2
RUN apt-get update && apt-get install -y apache2

# Step 3: Configure Apache to listen on port 8083
RUN sed -i 's/80/8083/' /etc/apache2/ports.conf
RUN sed -i 's/:80/:8083/' /etc/apache2/sites-available/000-default.conf

# Step 4: Expose port 8083
EXPOSE 8083

# Step 5: Start Apache in the foreground (recommended for Docker containers)
CMD ["apache2ctl", "-D", "FOREGROUND"]
```
### Step 2: Build the Docker Image

1. Save the Dockerfile in the /opt/docker/ directory.
2. Open a terminal on App Server 2 and navigate to the /opt/docker/ directory:
```bash
cd /opt/docker
```
3. Build the Docker image using the following command:
```bash
docker build -t nautilus-apache:latest .
```
This command will read the Dockerfile and create an image tagged nautilus-apache:latest.

### Step 3: Run the Docker Container
After the image is successfully built, you can run the container on port 8083 using the following command:
```bash
docker run -d -p 8083:8083 nautilus-apache:latest
```
This command will:

- Run the container in detached mode (-d).
- Bind port 8083 on the host to port 8083 on the container.

### Step 4: Verify the Apache Server
To verify that Apache is running and accessible on port 8083, open a web browser and go to:
```
http://<server-ip>:8083
```
or docker ps

### Screenshots

<img width="1050" height="527" alt="image" src="https://github.com/user-attachments/assets/9636488a-a8b0-4d44-94ac-153904ac2dca" />
<img width="1050" height="514" alt="image" src="https://github.com/user-attachments/assets/d159a940-ebc8-443a-9b3e-4c3f31f40ce8" />



