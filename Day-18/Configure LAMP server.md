# Day 18: Configure LAMP Server

xFusionCorp Industries is planning to host a WordPress website on their infra in Stratos Datacenter. They have already done infrastructure configuration—for example, on the storage server they already have a shared directory /vaw/www/html that is mounted on each app host under /var/www/html directory. Please perform the following steps to accomplish the task:

a. Install httpd, php and its dependencies on all app hosts.

b. Apache should serve on port 6200 within the apps.

c. Install/Configure MariaDB server on DB Server.

d. Create a database named kodekloud_db9 and create a database user named kodekloud_sam identified as password 8FmzjvFU6S. Further make sure this newly created user is able to perform all operation on the database you created.

e. Finally you should be able to access the website on LBR link, by clicking on the App button on the top bar. You should see a message like App is able to connect to the database using user kodekloud_sam

---

**Configuring a full **LAMP (Linux, Apache, MariaDB, PHP)** stack across multiple servers for xFusionCorp Industries in the Stratos Datacenter.**

## 📌 Objective

Deploy a WordPress-ready LAMP server setup using:

- Apache HTTP Server on App Servers (port **6200**)
- PHP with MySQL support
- MariaDB on a separate DB Server
- Shared directory `/vaw/www/html` mounted on `/var/www/html` in App servers
- Web app must confirm DB connectivity via a test message

---

## 🧩 Tasks Performed

### 🔹 App Server Configuration (App1, App2, App3)

1. **Install Apache and PHP:**

```bash
sudo yum install httpd php php-mysqlnd -y
Configure Apache to run on port 6200:

sudo sed -i 's/^Listen 80/Listen 6200/' /etc/httpd/conf/httpd.conf
sudo sed -i 's/<VirtualHost \*:80>/<VirtualHost *:6200>/' /etc/httpd/conf/httpd.conf
sudo systemctl restart httpd
sudo systemctl enable httpd
```
2. **Configure Apache to run on port 6200:**
```bash
sudo sed -i 's/^Listen 80/Listen 6200/' /etc/httpd/conf/httpd.conf
sudo sed -i 's/<VirtualHost \*:80>/<VirtualHost *:6200>/' /etc/httpd/conf/httpd.conf
sudo systemctl restart httpd
sudo systemctl enable httpd
```

3. **Verify Apache is listening:**
```bash
sudo ss -tuln | grep 6200
```
## 🔹 Database Server Configuration (DB Server - stdb01)

1. **Install MariaDB:**
```bash
sudo yum install mariadb-server -y
sudo systemctl start mariadb
sudo systemctl enable mariadb
```

2. **Configure remote access:**

Edit `/etc/my.cnf:`

```ini
[mysqld]
bind-address=0.0.0.0
```

Restart MariaDB:
```bash
sudo systemctl restart mariadb
```

3. **Create Database and User:**
```sql
CREATE DATABASE kodekloud_db9;
CREATE USER 'kodekloud_sam'@'%' IDENTIFIED BY '8FmzjvFU6S';
GRANT ALL PRIVILEGES ON kodekloud_db9.* TO 'kodekloud_sam'@'%';
FLUSH PRIVILEGES;
```
## 🌐 Application Test Script

A simple `index.php` was placed in `/var/www/html`:

<?php
$dbname = 'kodekloud_db9';
$dbuser = 'kodekloud_sam';
$dbpass = '8FmzjvFU6S';
$dbhost = 'stdb01';

$link = mysqli_connect($dbhost, $dbuser, $dbpass) or die("Unable to Connect to '$dbhost'");
echo "App is able to connect to the database using user $dbuser";
?>

## ✅ Final Output

Accessible via the Load Balancer URL (LBR):

http://<LBR_IP>:6200/


### Expected Message:

App is able to connect to the database using user kodekloud_sam

### 🚀 Skills

LAMP stack deployment

Multi-host configuration (App + DB separation)

Apache port customization

MySQL user privileges and remote access

Troubleshooting PHP-MySQL integration

### 📚 Tools Used

CentOS / RHEL

Apache HTTPD

PHP

MariaDB

systemd / firewalld

CLI tools (yum, vi, sed, mysql)

##
