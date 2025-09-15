# 🚀 KodeKloud Challenge – Day 16: Load Balancer Configuration

## ✅ Objective

To configure a high-availability Load Balancer (LBR) using **Nginx** to distribute traffic across 3 Apache-based application servers, each running on a **custom port (8089)**.

---

## 🧰 Environment

- **LBR Server**: `stlb01` (Load Balancer)
- **App Servers**: `stapp01`, `stapp02`, `stapp03`
- **Web Server**: Apache (httpd) running on **port 8089**
- **Load Balancer**: Nginx, listening on **port 80**

---

## 🛠️ Tasks Completed

### 1. ✅ Install Nginx on LBR Server

```bash
sudo yum install nginx -y     # RHEL/CentOS-based
sudo systemctl enable nginx
sudo systemctl start nginx
