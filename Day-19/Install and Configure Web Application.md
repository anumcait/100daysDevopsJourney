## Day 19: Install and Configure Web Application 

**Problem Statement:**

xFusionCorp Industries is planning to host two static websites on their infra in Stratos Datacenter. The development of these websites is still in-progress, but we want to get the servers ready. Please perform the following steps to accomplish the task: a. Install httpd package and dependencies on app server 2. b. Apache should serve on port 8088. c. There are two website's backups /home/thor/blog and /home/thor/demo on jump_host. Set them up on Apache in a way that blog should work on the link http://localhost:8088/blog/ and demo should work on link http://localhost:8088/demo/ on the mentioned app server. d. Once configured you should be able to access the website using curl command on the respective app server, i.e curl http://localhost:8088/blog/ and curl http://localhost:8088/demo/

### Web Application Setup for Static Websites on Apache HTTP Server

This project covers setting up two static websites on an Apache HTTP server, running on **port 8088**. The websites' content is backed up on the **jump_host** and needs to be transferred and configured on an app server.

## Steps:

### 1. Install Apache HTTP Server on App Server

Log in to the **app server** and install the Apache HTTP server.

#### For RHEL/CentOS:
```bash
sudo yum install httpd -y
```
### 2. Configure Apache to Listen on Port 8088

Edit the Apache configuration file to listen on port 8088.

### For RHEL/CentOS:

Edit the /etc/httpd/conf/httpd.conf file.
```bash
sudo nano /etc/httpd/conf/httpd.conf
```
Change the `Listen` directive to:
```apache
Listen 8088
```

### 3. Transfer Website Backups to the App Server

**set correct permission as it won't copy otherwise will get errors:**

```bash
sudo chown -R steve:steve /var/www/html/
```

**Use scp to transfer the website backups from the jump_host to the app server:**

```bash
scp -r /home/thor/blog steve@stapp02:/var/www/html/blog
scp -r /home/thor/demo steve@stapp02:/var/www/html/demo
```

### 6. Restart Apache Service

Restart Apache to apply the changes:

```bash
sudo systemctl restart httpd
```

### 7. Test the Configuration

Test the setup using curl on the app server:
```bash
curl http://localhost:8088/blog/
curl http://localhost:8088/demo/
```
##
