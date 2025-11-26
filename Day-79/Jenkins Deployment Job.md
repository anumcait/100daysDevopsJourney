Day-79: Jenkins Deployment Job

# Jenkins Deployment Job – Nautilus App

This guide provides step-by-step instructions to configure Jenkins for auto-deployment of the `web` repository from Gitea to the Storage Server under `/var/www/html`.

---

## **Prerequisites**

1. Jenkins UI access: `admin / Adm!n321`  
2. Gitea UI access: `sarah / Sarah_pass123`  
3. Storage Server access via SSH as user `sarah`.  
4. `httpd` installed on all app servers and serving on port 8080.  
5. Ensure `/var/www/html` is owned by `sarah`:

```bash
sudo chown -R sarah:sarah /var/www/html
sudo chmod -R 755 /var/www/html
```
## Step 1 – Install httpd on App Servers
```
sudo yum install -y httpd
sudo sed -i 's/^Listen .*/Listen 8080/' /etc/httpd/conf/httpd.conf
sudo systemctl enable httpd
sudo systemctl restart httpd
```
| Make sure port 8080 is open in firewall if required.

---

## Step 2 – Create Jenkins Job

1. Go to Jenkins → New Item → Enter nautilus-app-deployment → Freestyle Project → OK.
2. Source Code Management → Git:
  - Repository URL: http://git.stratos.xfusioncorp.com/sarah/web.git
  - Credentials: Add new → Username with private key
      - Username: sarah
      - Private Key: Paste content of /home/sarah/.ssh/id_rsa
      - Save
  - Branches to build: */master

## Step 3 – Configure Build Trigger

- Build Triggers → Check Poll SCM
- Schedule: H/5 * * * * (or any preferred frequency)

| This ensures Jenkins auto-builds when Git repo changes.

---

## Step 4 – Configure Publish Over SSH

Install plugin: Publish Over SSH via Jenkins Plugin Manager.

Manage Jenkins → Configure System → Publish over SSH

SSH Servers → Add:

Name: storage-server

Hostname: ststor01.stratos.xfusioncorp.com

Username: sarah

Remote Directory: /var/www/html

Use password authentication: No (we use SSH key)

SSH Key: paste content of /home/sarah/.ssh/id_rsa (no passphrase)

Test configuration → Should succeed

Step 5 – Add Build Step to Deploy

Build → Add build step → Send files or execute commands over SSH

Name: storage-server

Source files: **/* (or leave empty to send workspace root)

Remove prefix: /var/lib/jenkins/workspace/nautilus-app-deployment

Exec command:
```
#!/bin/bash
set -e
# Clean old content
rm -rf /var/www/html/*
# Copy latest code
cp -r /var/lib/jenkins/workspace/nautilus-app-deployment/* /var/www/html/
# Set permissions
chown -R sarah:sarah /var/www/html
chmod -R 755 /var/www/html
```

---

## Step 6 – Save & Test Job

Save the job.

Click Build Now.

Ensure status is Success.

Verify /var/www/html on Storage Server contains the latest repo content.

## Step 7 – Update Repository

SSH into storage server as sarah:
```
ssh sarah@ststor01.stratos.xfusioncorp.com
cd ~/web
echo "Welcome to the xFusionCorp Industries" > index.html
git add index.html
git commit -m "Update welcome message"
git push origin master
```

Jenkins job should trigger automatically and deploy the new content to /var/www/html.

---

## Step 8 – Verify Deployment

Open App → https://<LBR-URL>

Ensure latest index.html content is visible.

No sub-directory like /web should appear.

## Notes

Ensure httpd is running on port 8080 on all app servers:
```
sudo systemctl status httpd
```

- Jenkins job should pass on repeated runs.
- All deployment actions should run as sarah to avoid permission issues.
- If using a new SSH key, ensure correct private key format (PEM) and no passphrase.

---

## Screenshots

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/62a6a890-5c07-4ac6-8fa1-2b18f11d7af6" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/f97033d9-017d-4d6c-ab6c-d0c2d5d69c1b" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/b3914330-47a2-4738-9b52-fdb8f77bbc6b" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/6f943b8f-04ad-4059-9172-3546f8649c44" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/c2f3aec9-888e-4442-83e9-4c1434918615" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/38901858-0034-4584-a929-5c7c66a381b7" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/2b82d45b-6038-4cad-bb08-01eb320764c6" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/975a3b05-3124-40bd-a86d-9cec88ce58b1" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/72c7de70-2b6c-44b3-95cc-fef4b317e763" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/86a6d581-7e90-4f87-8434-e714d52fc06f" />











