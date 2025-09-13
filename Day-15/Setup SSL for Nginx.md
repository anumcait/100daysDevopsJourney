Day 15: Setup SSL for Nginx

Proble Statement:

The system admins team of xFusionCorp Industries needs to deploy a new application on App Server 2 in Stratos Datacenter. They have some pre-requites to get ready that server for application deployment. Prepare the server as per requirements shared below:



1. Install and configure nginx on App Server 2.


2. On App Server 2 there is a self signed SSL certificate and key present at location /tmp/nautilus.crt and /tmp/nautilus.key. Move them to some appropriate location and deploy the same in Nginx.


3. Create an index.html file with content Welcome! under Nginx document root.


4. For final testing try to access the App Server 2 link (either hostname or IP) from jump host using curl command. For example curl -Ik https://<app-server-ip>/.


# 🚀 Day 15 - Setup SSL for Nginx (KodeKloud 100 Days of DevOps)

## 🔧 Task Overview

The system admins team of xFusionCorp Industries needs to deploy a new application on App Server 2. Before deployment, the server must be prepared with Nginx and SSL. Here's what was done:

### ✅ Requirements:

1. Install and configure **Nginx** on App Server 2
2. Move the provided self-signed SSL certificate and key:
   - From: `/tmp/nautilus.crt`, `/tmp/nautilus.key`
   - To: `/etc/nginx/ssl/`
3. Configure Nginx to use the SSL certificate and serve a basic `index.html`
4. Verify the HTTPS response from the Jump Host using `curl`

---

## 🧰 Step-by-Step Instructions

### 1️⃣ Install Nginx

```bash
sudo yum install nginx -y         # CentOS/RHEL
# or
sudo apt update && sudo apt install nginx -y   # Ubuntu/Debian
