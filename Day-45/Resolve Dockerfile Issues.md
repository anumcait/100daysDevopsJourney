# Day-45: Resolve Dockerfile Issues

## Problem Statement

The Nautilus DevOps team is working to create new images per requirements shared by the development team. One of the team members is working to create a Dockerfile on App Server 3 in Stratos DC. While working on it she ran into issues in which the docker build is failing and displaying errors. Look into the issue and fix it to build an image as per details mentioned below:

a. The Dockerfile is placed on App Server 3 under /opt/docker directory.

b. Fix the issues with this file and make sure it is able to build the image.

c. Do not change base image, any other valid configuration within Dockerfile, or any of the data been used — for example, index.html.

Note: Please note that once you click on FINISH button all the existing containers will be destroyed and new image will be built from your Dockerfile.

## Solution
Your task is to fix a broken Dockerfile on App Server 3 under /opt/docker without changing the base image or altering other configurations or data like index.html.

## ✅ Step-by-Step Solution

### 1. Log in to App Server 3

- you’ll be SSH’ing into App Server 3. Use the correct credentials provided by the lab environment.
  - ssh banner@stapp03
### 2. Navigate to the Dockerfile directory
```bash
cd /opt/docker
```
### 3. List the contents to confirm presence of Dockerfile and required files
```bash
ls -l
```
You should see:

- Dockerfile
- Possibly index.html or other files used by Dockerfile

### 4. Check contents of the Dockerfile
```bash
cat Dockerfile
```
### 5. Correct the Dockerfile

```bash
FROM httpd:2.4.43

# Change Apache to listen on port 8080
RUN sed -i "s/Listen 80/Listen 8080/g" /usr/local/apache2/conf/httpd.conf

# Enable SSL modules and config
RUN sed -i '/LoadModule ssl_module modules\/mod_ssl.so/s/^#//g' /usr/local/apache2/conf/httpd.conf && \
    sed -i '/LoadModule socache_shmcb_module modules\/mod_socache_shmcb.so/s/^#//g' /usr/local/apache2/conf/httpd.conf && \
    sed -i '/Include conf\/extra\/httpd-ssl.conf/s/^#//g' /usr/local/apache2/conf/httpd.conf

# Copy SSL certs
COPY certs/server.crt /usr/local/apache2/conf/server.crt
COPY certs/server.key /usr/local/apache2/conf/server.key

# Copy website content
COPY html/index.html /usr/local/apache2/htdocs/

```
## 🧪 Build the Docker Image
Run the following from /opt/docker:
```bash
docker build -t my-httpd-app .
```
You should see:
```bash
Successfully built <image-id>
Successfully tagged my-httpd-app:latest
```
## ✅ Final Notes

- Don't change the base image → ✅ httpd:2.4.43 is preserved.
- Don't change index.html or certs → ✅ All kept intact.
- Fix only the Dockerfile syntax and logic → ✅ Done.

Once confirmed working, you can safely hit FINISH per the instructions.

## Screen shots
<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/e147fe18-a99e-4c14-b470-b320078f2af9" />

<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/6ab71720-61df-4125-8222-9c892ea25a9b" />



 



