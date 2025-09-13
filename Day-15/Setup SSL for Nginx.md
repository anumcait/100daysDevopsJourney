# 🚀 Day 15 - Setup SSL for Nginx

## 🔧 Task Overview

The system admins team of xFusionCorp Industries needs to deploy a new application on App Server 2 in Stratos Datacenter. They have some pre-requites to get ready that server for application deployment. Prepare the server as per requirements shared below:

1. Install and configure nginx on App Server 2.

2. On App Server 2 there is a self signed SSL certificate and key present at location /tmp/nautilus.crt and /tmp/nautilus.key. Move them to some appropriate location and deploy the same in Nginx.

3. Create an index.html file with content Welcome! under Nginx document root.

4. For final testing try to access the App Server 2 link (either hostname or IP) from jump host using curl command. For example curl -Ik https://<app-server-ip>/.

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
```
### 2️⃣ Create SSL Directory & Move Certs
```bash
sudo mkdir -p /etc/nginx/ssl
sudo mv /tmp/nautilus.crt /etc/nginx/ssl/
sudo mv /tmp/nautilus.key /etc/nginx/ssl/
sudo chmod 600 /etc/nginx/ssl/nautilus.key
```
### 3️⃣ Create Web Root and Index Page
```bash
sudo mkdir -p /var/www/html
echo "Welcome!" | sudo tee /var/www/html/index.html
```
Adjust permissions if needed:

```bash
sudo chown -R nginx:nginx /var/www/html
```
4️⃣ Update Nginx Configuration

Edit the main config:
```bash
sudo nano /etc/nginx/nginx.conf
```
Make sure your config is inside the http block:
```bash
http {
    ...
    server {
        listen 443 ssl;
        server_name localhost;

        ssl_certificate /etc/nginx/ssl/nautilus.crt;
        ssl_certificate_key /etc/nginx/ssl/nautilus.key;

        root /var/www/html;
        index index.html;

        location / {
            try_files $uri $uri/ =404;
        }
    }

    server {
        listen 80;
        return 301 https://$host$request_uri;
    }
}
```
### 5️⃣ Test and Restart Nginx

```bash
sudo nginx -t        # Test config
sudo systemctl restart nginx
sudo systemctl enable nginx
```
## 🔍 Validation from Jump Host

On the **Jump Host**, test the HTTPS endpoint:

curl -Ik https://<app-server-2-ip>

Expected Output:
```
HTTP/1.1 200 OK
Server: nginx/...
Content-Type: text/html
...
```
If using a self-signed cert, bypass SSL check with:

curl -k https://<app-server-2-ip>

## 📦 Directory Structure

/etc/nginx/nginx.conf        # Main Nginx config file
/etc/nginx/ssl/              # SSL certificate and key
/var/www/html/index.html     # Web root with welcome page

📘 Useful Commands
# Check Nginx service
sudo systemctl status nginx

# Reload Nginx after changes
sudo nginx -s reload

# View logs if troubleshooting
sudo tail -f /var/log/nginx/error.log

## ✅ Outcome

🔐 HTTPS successfully configured using a self-signed certificate

🖥️ Static web page served via Nginx on port 443

🧪 Verified with curl from Jump Host
