# **Day 40: Docker EXEC Operations**

## Problem Statement:
One of the Nautilus DevOps team members was working to configure services on a kkloud container that is running on App Server 1 in Stratos Datacenter. 

Due to some personal work he is on PTO for the rest of the week, but we need to finish his pending work ASAP. Please complete the remaining work as per details given below: 

a. Install apache2 in kkloud container using apt that is running on App Server 1 in Stratos Datacenter. 

b. Configure Apache to listen on port 8089 instead of default http port. Do not bind it to listen on specific IP or hostname only, i.e it should listen on localhost, 127.0.0.1, container ip, etc. 

c. Make sure Apache service is up and running inside the container. Keep the container in running state at the end.

## **Overview**
This document outlines the steps to install Apache2 on the `kkloud` container running on **App Server 1** in the **Stratos Datacenter**. The tasks also include configuring Apache to listen on port 8089 and ensuring the Apache service is running properly.

## **Prerequisites**
- You need access to the `kkloud` container on App Server 1.
- You should have root or `sudo` privileges to install packages and configure the system.

## **Steps to Complete the Tasks**

### 1. **Access the `kkloud` Container**

To access the container running on App Server 1, run the following command (replace `kkloud` with the actual container name if it's different):

```bash
sudo docker exec -it kkloud bash
```
### 2. Update the Package List
Once inside the container, update the apt package lists to make sure you have the latest information:

```bash
apt update
```
### 3. Install Apache2
Now, install Apache2 using the apt package manager:
```bash
apt install -y apache2
```
### 4. Configure Apache to Listen on Port 8089

Edit ports.conf
Open the ports.conf file where the listening ports are defined:
```bash
nano /etc/apache2/ports.conf
Add the line to make Apache listen on port 8089:
```
Listen 8089
Edit 000-default.conf
Open the default site configuration file to change the port for the virtual host:

```bash
nano /etc/apache2/sites-available/000-default.conf
```
Modify the <VirtualHost> directive to use port 8089:

```bash
<VirtualHost *:8089>
```
### 5. Set ServerName to Suppress Warnings
To suppress the "Could not reliably determine the server's fully qualified domain name" warning, edit the apache2.conf file:

Open the apache2.conf file:

```bash
nano /etc/apache2/apache2.conf
```
Add the following line at the end of the file:

ServerName localhost

### 6. Restart Apache Service
To apply the changes, restart the Apache service:
```bash
service apache2 restart
```
### 7. Verify Apache is Running
Check if Apache is running using the ps command:

```bash
ps aux | grep apache2
```
You should see Apache processes listed.

### 8. Verify Apache is Listening on Port 8089
Use the following command to verify that Apache is listening on port 8089:

```bash
ss -tuln | grep 8089
```
If Apache is correctly listening on port 8089, you should see an output similar to this:

```
LISTEN   0         128              *:8089              *:*
```
### 9. Test Apache is Serving on Port 8089
You can test whether Apache is serving content on port 8089 by using curl:

```bash
curl http://localhost:8089
```

If Apache is working correctly, you should see the Apache default page or any other content you have configured.

### 10. Ensure the Container Remains Running

To ensure the container stays up and running:
If you're using interactive mode (docker exec -it), simply exit the session, and the container will remain running.

exit

You can verify the container is still running with:

docker ps


<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/cf87510b-5367-4231-96e9-f1b1a2b0bbfd" />
