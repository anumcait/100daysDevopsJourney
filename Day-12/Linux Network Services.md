# Day 12 - Apache Service Troubleshooting on Stratos Datacenter

## Overview

This task involved troubleshooting an issue with the Apache HTTP server (`httpd`) on the app server `stapp01` in the Stratos Datacenter. The Apache service was not reachable on the custom port **6300**, which was affecting application availability.

---

## Problem Statement

Our monitoring tool reported that the Apache service on `stapp01` was unreachable on port `6300`. The issue could have been due to:
- Apache service being down
- Firewall blocking the port
- Port conflicts or other networking issues

The goal was to identify and fix the root cause, ensuring Apache was reachable from the jump host on port 6300 without modifying any existing web content.

---

## Troubleshooting Steps

### 1. Check Apache service status

```bash
sudo systemctl status httpd
Found that the Apache service failed to start.
```
### 2. Review error logs
```bash
sudo journalctl -xe
```
Error indicated:
Address already in use: AH00072: make_sock: could not bind to address 0.0.0.0:6300
This means the port was already occupied by another process.

### 3. Identify process using port 6300
```bash
sudo netstat -tulnp | grep 6300
```
Discovered that sendmail service was listening on port 6300 on 127.0.0.1.

### 4. Resolve port conflict

Since Apache needed port 6300 on all interfaces (0.0.0.0), but sendmail was using it on localhost, this prevented Apache from binding properly.

Decided to stop and disable sendmail since it was not required for this application server:
```bash
sudo systemctl stop sendmail
sudo systemctl disable sendmail
```
### 5. Restart Apache service
```bash
sudo systemctl start httpd
```
Apache started successfully.

### 6. Confirm Apache is listening on port 6300
```bash
sudo netstat -tulnp | grep 6300
```
Confirmed Apache (httpd) process was now listening on 0.0.0.0:6300.

### 7. Check and update firewall settings

Verified firewall was allowing traffic on port 6300:
```bash
sudo firewall-cmd --list-all
```

Opened port 6300 if it was not already allowed:
```bash
sudo firewall-cmd --add-port=6300/tcp --permanent
sudo firewall-cmd --reload
```

### 8. Test connectivity from jump host

From the jump host, ran:
```bash
curl http://stapp01:6300
```
Received a successful HTTP response confirming the Apache service was reachable.

### Outcome

- Successfully identified that port 6300 was occupied by the sendmail service.

- Freed the port by stopping and disabling sendmail.

- Restarted Apache and ensured it was listening on port 6300.

- Verified and adjusted firewall settings to allow traffic on port 6300.

- Confirmed connectivity from the jump host using curl.

No changes were made to the existing web content or security settings, ensuring compliance with task requirements.

- Tools and Commands Used
- Tool/Command	Purpose
- systemctl	Manage services (check status, start, stop)
- journalctl	View system and service logs
- netstat / ss	Check open ports and process bindings
- firewall-cmd	Manage firewall rules
- curl	Test HTTP connectivity from jump host
- grep	Search text within files and outputs
