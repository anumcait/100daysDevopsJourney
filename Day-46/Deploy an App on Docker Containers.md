# Day 46: Deploy an App on Docker Containers

Task Definition:
The Nautilus Application development team recently finished development of one of the apps that they want to deploy on a containerized platform. The Nautilus Application development and DevOps teams met to discuss some of the basic pre-requisites and requirements to complete the deployment. The team wants to test the deployment on one of the app servers before going live and set up a complete containerized stack using a docker compose fie. Below are the details of the task:

On App Server 2 in Stratos Datacenter create a docker compose file /opt/itadmin/docker-compose.yml (should be named exactly).

The compose should deploy two services (web and DB), and each service should deploy a container as per details below:

For web service: 

a. Container name must be php_web.

b. Use image php with any apache tag. Check here for more details. 

c. Map php_web container's port 80 with host port 6400 

d. Map php_web container's /var/www/html volume with host volume /var/www/html. 


For DB service: 

a. Container name must be mysql_web. 

b. Use image mariadb with any tag (preferably latest). Check here for more details. 

c. Map mysql_web container's port 3306 with host port 3306 

d. Map mysql_web container's /var/lib/mysql volume with host volume /var/lib/mysql. 

e. Set MYSQL_DATABASE=database_web and use any custom user ( except root ) with some complex password for DB connections. 


After running docker-compose up you can access the app with curl command curl <server-ip or hostname>:6400/ For more details check here. 

Note: Once you click on FINISH button, all currently running/stopped containers will be destroyed and stack will be deployed again using your compose file.

## Solution

In this task, we will deploy a containerized stack using Docker Compose on App Server 2 in the Stratos Datacenter. The stack will consist of two services: a web service using PHP with Apache and a database service using MariaDB.

## Prerequisites

- You must have access to App Server 2 in Stratos Datacenter.
- Docker and Docker Compose must be installed on App Server 2.

## Step-by-Step Guide

### 1. SSH into App Server 2

First, SSH into App Server 2 where you will create and deploy the Docker Compose stack.

```bash
ssh your-user@<app-server-2-ip>
```
### 2. Create the docker-compose.yml File

Create the directory /opt/itadmin (if it doesn't already exist), and navigate to it.
```bash
sudo mkdir -p /opt/itadmin
```
Now create the docker-compose.yml file.

```bash
sudo nano /opt/itadmin/docker-compose.yml
```
3. Add the Compose Configuration

In the docker-compose.yml file, add the following content to define both the web and db services.
```bash
version: '3.8'

services:
  web:
    container_name: php_web
    image: php:apache
    ports:
      - "6400:80"
    volumes:
      - /var/www/html:/var/www/html

  db:
    container_name: mysql_web
    image: mariadb:latest
    environment:
      MYSQL_DATABASE: database_web
      MYSQL_USER: custom_user
      MYSQL_PASSWORD: complexpassword123
      MYSQL_ROOT_PASSWORD: rootpassword123
    ports:
      - "3306:3306"
    volumes:
      - /var/lib/mysql:/var/lib/mysql
```
**Explanation:**

- **Web Service (php_web):**

  - Uses php:apache image (official PHP with Apache).
  - Maps container port 80 to host port 6400.
  - Mounts /var/www/html directory from host to container for content.

- **DB Service (mysql_web):**

  - Uses mariadb:latest image (MariaDB, a MySQL drop-in replacement).
  - Environment variables set:
    - MYSQL_DATABASE creates a database database_web.
    - MYSQL_USER creates a custom user.
    - MYSQL_PASSWORD is the password for the custom user.
    - MYSQL_ROOT_PASSWORD sets the root password.

  - Maps container port 3306 to host port 3306.
  - Mounts /var/lib/mysql from host to container to persist database data.

### 4. Save and Exit

After adding the content, save and close the file:

Press CTRL + X to close.

Press Y to confirm saving.

Press Enter to save with the same name.

### 5. Run Docker Compose

To start the services, use the following command:

```bash
cd /opt/itadmin
sudo docker-compose up -d
```
This will:

Pull the required Docker images (php:apache and mariadb:latest).

Start the containers in detached mode.

### 6. Verify the Deployment

To check if the containers are running, use the following command:
```bash
sudo docker ps
```
### 7. Test the Web Application

You can now access the web service using curl to verify that it's running:
```bash
curl <server-ip-or-hostname>:6400/
```

### Screenshots

<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/13cd66e5-888c-487f-ba62-2e9653da81e5" />

<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/6c4c2007-15cd-4021-83de-14a920f9d06a" />



