# Day 20: Configure Nginx + PHP-FPM Using Unix Sock

**Problem Statement**

The Nautilus application development team is planning to launch a new PHP-based application, which they want to deploy on Nautilus infra in Stratos DC. The development team had a meeting with the production support team and they have shared some requirements regarding the infrastructure. Below are the requirements they shared: a. Install nginx on app server 1 , configure it to use port 8097 and its document root should be /var/www/html. b. Install php-fpm version 8.1 on app server 1, it must use the unix socket /var/run/php-fpm/default.sock (create the parent directories if don't exist). c. Configure php-fpm and nginx to work together.

---

This repository contains the configuration and setup instructions for deploying a PHP application on a Linux server using **Nginx** and **PHP-FPM 8.1** connected via a Unix socket for better performance.

---

## Features

- Nginx listening on port `8097`
- Document root set to `/var/www/html`
- PHP-FPM configured to use Unix socket: `/var/run/php-fpm/default.sock`
- Secure socket permissions for proper communication between Nginx and PHP-FPM
- Tested with sample PHP files (`index.php` and `info.php`)

---

## Prerequisites

- CentOS/RHEL 8 or compatible Linux distribution
- Root or sudo privileges
- PHP 8.1 and Nginx installed

---

## Setup Instructions

### 1. Install Nginx and PHP-FPM 8.1

```bash
sudo dnf module reset php
sudo dnf module enable php:8.1 -y
sudo dnf install nginx php-fpm -y
```
### 2. Configure PHP-FPM to use Unix Socket


Edit /etc/php-fpm.d/www.conf: 

```bash
listen = /var/run/php-fpm/default.sock

```

Create the socket directory if not exists:
```bash
sudo mkdir -p /var/run/php-fpm
sudo chown -R nginx:nginx /var/run/php-fpm
```
### 3. Configure Nginx to listen on port 8097 and use the Unix socket

Create /etc/nginx/conf.d/phpapp.conf with:
```bash
server {
    listen 8097;
    server_name stapp01;

    root /var/www/html;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include fastcgi_params;
        fastcgi_pass unix:/var/run/php-fpm/default.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }
}
```
### 4. Start and enable services

```bash
sudo systemctl enable --now php-fpm
sudo systemctl enable --now nginx
```
### 5. Test your setup

From your jump host or locally:

```bash
curl http://stapp01:8097/index.php
```
You should get like below screenshot
<img width="1280" height="650" alt="image" src="https://github.com/user-attachments/assets/87df27c5-69d9-4468-9705-851cbcc16587" />

##

