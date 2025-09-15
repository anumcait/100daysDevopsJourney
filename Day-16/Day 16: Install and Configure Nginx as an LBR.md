# 🚀 KodeKloud Challenge – Day 16: Load Balancer Configuration

## ✅ Objective

To configure a high-availability Load Balancer (LBR) using **Nginx** to distribute traffic across 3 Apache-based application servers, each running on a **custom port (8089)**.

---

## 🧰 Environment

- **LBR Server**: `stlb01` (Load Balancer)
- **App Servers**: `stapp01`, `stapp02`, `stapp03`
- **Web Server**: Apache (httpd) running on **port 5004**
- **Load Balancer**: Nginx, listening on **port 80**

---

## 🛠️ Tasks Completed

### 1. ✅ Install Nginx on LBR Server

```bash
sudo yum install nginx -y     # RHEL/CentOS-based
sudo systemctl enable nginx
sudo systemctl start nginx
```
### 2. ✅ Verify Apache on All App Servers

On stapp01, stapp02, and stapp03:
```bash
sudo systemctl status httpd
sudo ss -tuln | grep 8089
```

If Apache wasn't listening on port 8089, it was fixed by:

sudo vi /etc/httpd/conf/httpd.conf
# Changed: Listen 80 → Listen 8089
sudo systemctl restart httpd

3. ✅ Update Nginx Configuration

Edited the main config file: /etc/nginx/nginx.conf
```bash
http {
    upstream backend_servers {
        server stapp01:8089;
        server stapp02:8089;
        server stapp03:8089;
    }

    server {
        listen 80;
        server_name localhost;

        location / {
            proxy_pass http://backend_servers;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
    }
}
```
### 4. ✅ Restart and Test Nginx
```bash
sudo nginx -t
sudo systemctl restart nginx
```
### 5. ✅ Validate Setup

Accessed the application via StaticApp in the KodeKloud environment. Successfully served traffic through the load balancer to all App Servers running on port 8089.

### 📌 Key Learnings

Dealing with non-default Apache ports in real-world infra

Troubleshooting 502 Bad Gateway (connectivity, ports, services)

Load balancing with Nginx upstream configuration

Hands-on with end-to-end service chaining

### 📎 Tools Used

- Nginx

- Apache (httpd)

- systemd (service management)

- netstat / ss (port verification)

- curl (connectivity testing)
