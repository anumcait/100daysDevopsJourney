# Day 80: Jenkins Chained Builds

This document explains how to configure **Jenkins chained builds** to deploy the Nautilus web app and restart Apache services on all app servers in the Stratos Datacenter.

---

- **Goal:** Deploy latest web content to shared `/var/www/html` and restart Apache on all app servers automatically.
- **Servers:**

| Server | Role | Notes |
|--------|------|-------|
| `jenkins.stratos.xfusioncorp.com` | Jenkins server | Runs CI/CD jobs |
| `ststor01` | Storage server | `/var/www/html` is a Git repo shared to app servers |
| `stapp01-03` | App servers | Apache installed, shared volume mounted |
| `stlb01` | Load balancer | Serves app content at root URL |

- **Git Repository:** Gitea repo `web` under user `sarah`

---

## **Step 1: Install Required Jenkins Plugins**

1. Log in to Jenkins: `admin / Adm!n321`
2. Navigate to: `Manage Jenkins → Manage Plugins → Available`
3. Install the following plugins:
   - **Git plugin**
   - **Git client plugin**
   - **Parameterized Trigger plugin**
   - **SSH Agent / SSH Steps plugin**
4. Restart Jenkins safely after installation.

---

## **Step 2: Create Upstream Job (`nautilus-app-deployment`)**

1. **New Item → Freestyle project → `nautilus-app-deployment`**
2. **Source Code Management → Git**
   - Repository URL: `http://172.16.238.15:8090/sarah/web.git`
   - Credentials: `sarah / Sarah_pass123`
   - Branch: `master`
3. **Build Step → Execute Shell**

```bash
# Pull latest code on storage server
sshpass -p 'Bl@kW' ssh -o StrictHostKeyChecking=no natasha@ststor01 \
"cd /var/www/html && git pull origin master"
```
4. **Post-build Action → Build other projects**

- Projects to build: manage-services
- Trigger only if build is stable

5. ** Save the Job**
---

## Step 3: Create Downstream Job (manage-services)

New Item → Freestyle project → manage-services

Build Step → Execute Shell
```
# Restart Apache on all app servers
sshpass -p 'Ir0nM@n' ssh -o StrictHostKeyChecking=no tony@stapp01 "echo 'Ir0nM@n' | sudo -S systemctl restart httpd"
sshpass -p 'Am3ric@' ssh -o StrictHostKeyChecking=no steve@stapp02 "echo 'Am3ric@' | sudo -S systemctl restart httpd"
sshpass -p 'BigGr33n' ssh -o StrictHostKeyChecking=no banner@stapp03 "echo 'BigGr33n' | sudo -S systemctl 
```

| Note: Using sudo -S allows password to be provided non-interactively since Jenkins runs SSH commands without a TTY.

3. Save the job.

---

## Step 4: Test the Pipeline

1. Build nautilus-app-deployment manually.

2. Verify:

  - Git repository is updated on ststor01:
    ```
    ssh natasha@ststor01 "cd /var/www/html && git log -1"
    ```
  - Downstream job is triggered automatically.
  - Apache is running on all app servers:
    ```
    ssh tony@stapp01 "systemctl status httpd"
    ssh steve@stapp02 "systemctl status httpd"
    ssh banner@stapp03 "systemctl status httpd"
    ```
3. Open the LB URL: https://<LBR-URL>
  - Latest index.html should appear at root, no /web subdirectory.

---

## Step 5: Notes

- Use sshpass for non-interactive SSH authentication.
- Ensure /var/www/html on the storage server has proper permissions for natasha.
- Jobs must pass on repeated builds.
- Document or record UI changes for lab validation.

---

## Screenshots

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/0f36905a-101a-4c51-81ac-7275270ef71b" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/034fc2eb-cf8e-40e7-8459-32f86d92e27d" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/27fec8ee-7a87-404f-b1cb-6a5d1f78aad3" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/b2e4750c-833d-4f28-a595-d13b33028134" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/a0ed1afe-b4cf-49c2-aac2-06294de274e9" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/4893eee0-8caa-4084-be93-d3971576a871" />

---












