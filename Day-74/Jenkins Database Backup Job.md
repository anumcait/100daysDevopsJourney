# Day-74 : Jenkins Database Backup Job

## Goal:
Automate MySQL backups from a DB server (stdb01) to a backup server (stbkp01) using Jenkins, either with sshpass or SSH keys.

## Step-by-Step Solution
### 1. Access Jenkins UI
1. Click the Jenkins button in the top bar of the lab.
2. Login using:
  - Username: admin
  - Password: Adm!n321

---

### 2. Create the Jenkins Job

- On the Jenkins dashboard, click New Item.
- Enter item name: database-backup
- Select Freestyle project → Click OK

---

### 3. Configure the Job – Shell Script

Scroll down to Build → Add build step → Execute shell

Add this script (adjust hostnames if your lab requires):
```
#!/bin/bash

# Database credentials
DB_NAME="kodekloud_db01"
DB_USER="kodekloud_roy"
DB_PASS="asdfgdsd"

# Output file
BACKUP_FILE="db_$(date +%F).sql"

# Dump DB from the Database Server
mysqldump -h database.stratos.xfusioncorp.com -u${DB_USER} -p${DB_PASS} ${DB_NAME} > /tmp/${BACKUP_FILE}

# Copy dump to Backup Server
scp /tmp/${BACKUP_FILE} clint@backup.stratos.xfusioncorp.com:/home/clint/db_backups/
```

### 4. Add Build Schedule (CRON)

Scroll to Build Triggers → Check Build periodically

Enter exactly:
```
*/10 * * * *
```

This schedules the job to run every 10 minutes.

---

### 5. Save The Job

Click Save

### 6. Run the Job for Testing

Click Build Now

Then check:

Console Output → ensure mysqldump + scp work correctly.

On the Backup Server, verify the file:

ls -l /home/clint/db_backups/

File should look like:

db_2025-11-19.sql

---

## Screenshots:

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/46913018-bea7-40ed-bd30-407a9cbae675" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/baeac6ab-d937-42f2-b135-08ca9a5eeb66" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/98781023-f423-49a3-9abd-58cb91114506" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/bc7933bf-5143-4470-81a9-af107df3d95d" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/9e58e34a-86af-412c-a332-97468ee3d2ee" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/8580a527-e0e2-40b3-9c02-789e1f34721d" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/39b64e89-3665-4c21-86a4-582495606c58" />


---




