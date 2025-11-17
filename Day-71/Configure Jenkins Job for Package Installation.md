# Day 71: Configure Jenkins Job for Package Installation

Problem Statement:
```
Some new requirements have come up to install and configure some packages on the Nautilus infrastructure under Stratos Datacenter. The Nautilus DevOps team installed and configured a new Jenkins server so they wanted to create a Jenkins job to automate this task. Find below more details and complete the task accordingly:
1. Access the Jenkins UI by clicking on the Jenkins button in the top bar. Log in using the credentials: username admin and password Adm!n321.
2. Create a new Jenkins job named install-packages and configure it with the following specifications:
Add a string parameter named PACKAGE.
Configure the job to install a package specified in the $PACKAGE parameter on the storage server within the Stratos Datacenter.
Note:
1. Ensure to install any required plugins and restart the Jenkins service if necessary. Opt for Restart Jenkins when installation is complete and no jobs are running on the plugin installation/update page. Refresh the UI page if needed after restarting the service.
2. Verify that the Jenkins job runs successfully on repeated executions to ensure reliability.
3. Capture screenshots of your configuration for documentation and review purposes. Alternatively, use screen recording software like loom.com for comprehensive documentation and sharing.
```
---

# Day 71 – Configure Jenkins Job for Package Installation
**KodeKloud 100 Days of DevOps Challenge**

This task focuses on creating a Jenkins job that automatically installs packages on the Storage Server inside the Stratos Datacenter. The job must take a package name as a parameter and install it remotely via SSH.

---

## ✅ 1. Access Jenkins

1. Click the **Jenkins** button in the top navigation bar of the lab environment.
2. Log in with the provided credentials:
   - **Username:** `admin`
   - **Password:** `Adm!n321`

---

## ✅ 2. Create a New Jenkins Job

1. On the Jenkins dashboard, click **New Item**.
2. Enter job name: **`install-packages`**
3. Select **Freestyle Project**
4. Click **OK**

---

## ✅ 3. Add Job Parameter

1. Check **This project is parameterized**
2. Click **Add Parameter → String Parameter**
3. Configure the parameter:

   - **Name:** `PACKAGE`
   - **Default Value:** `htop` (optional)
   - **Description:** Package to install on the Storage Server.

---

## ✅ 4. Add Build Step to Install the Package

1. Scroll to the **Build** section.
2. Click **Add build step → Execute shell**
3. Paste the following script:

```bash
echo "Installing package: $PACKAGE on storage server"

sshpass -p 'Stor@ge!23' ssh -o StrictHostKeyChecking=no natasha@ststor01 "
  echo 'Stor@ge!23' | sudo -S yum install -y $PACKAGE 2>/dev/null || \
  echo 'Stor@ge!23' | sudo -S apt-get install -y $PACKAGE 2>/dev/null
"
```

---

#### 🔍 Notes

- sshpass bypasses interactive password entry.
- sudo -S lets Jenkins pass the sudo password non-interactively.
- Supports both yum and apt based servers.

### ✅ 5. Install Required Plugins (If Needed)

If Jenkins shows SSH-related errors, install:

- SSH Agent Plugin
- Credential Binding Plugin
- SSH Pipeline Steps (optional)
After installation:
- Click Restart Jenkins when installation is complete
- Refresh the browser after restart.

---


### ✅ 6. Run the Jenkins Job

1. Click Build with Parameters
2. Enter the package name (examples: htop, tree, git)
3. Click Build

**✔ Expected Console Output**
```
Installing package: htop on storage server
Complete!
```

The build result should be SUCCESS.

---

### 🔄 7. Verify Job Reliability

Run the job multiple times to confirm:

- No errors occur when the package is already installed.
- SSH and sudo commands run smoothly.
- Jenkins accepts password via stdin (non-interactive).

---

#### Screenshots

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/1228f810-ba4f-4c17-bb78-f7bf4af74332" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/580a54a4-7a6c-40e3-b786-51cff73882be" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/666f02a1-a926-4497-b056-5467a1c5cc91" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/d82bf233-860d-4b18-86c3-d2016bc9f0ea" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/15080a5f-2847-4a54-93ab-240f744dece4" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/90efa8da-d9f0-43a7-9849-45c5b894d924" />







