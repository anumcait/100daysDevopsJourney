# Day 73: Jenkins Scheduled Jobs

This task configures a Jenkins freestyle job that runs every 10 minutes and copies Apache logs from App Server 3 to the Storage Server.

---

## 1. Login to Jenkins
- Access Jenkins UI
- Login:
  - Username: admin
  - Password: Adm!n321

---

## 2. Install Required Plugins
Navigate to:

| Manage Jenkins → Manage Plugins → Available


Install:
- Publish Over SSH
- SSH Agent
- SSH Pipeline Steps

Restart Jenkins when prompted.

---

## 3. Configure SSH for Storage Server

Navigate to:

| Manage Jenkins → Configure System

```
Under **Publish over SSH**, add:

- Name: storage-server  
- Hostname: ststor01  
- Username: natasha  
- Password: <storage server password>  
- Remote Directory: /usr/src/finance  

Click **Test Configuration**.
```
---

## 4. Verify Apache Logs on App Server 3

On the jump host:

```bash
ssh banner@stapp03
cd /var/log/httpd/
ls
```

Expected:

access_log
error_log

## 5. Create Jenkins Freestyle Job

Navigate to:

| New Item → Freestyle Project

```
Name the job:

copy-logs

```
Click OK.

## 6. Configure Build Trigger

Under Build Triggers, enable:

Build periodically


Enter:

*/10 * * * *

## 7. Add Post-Build Action

Navigate to Post-build Actions:

Add post-build action → Send build artifacts over SSH


Choose storage-server.

In Exec command, enter:
```
sshpass -p 'BANNER_PASSWORD' scp -o StrictHostKeyChecking=no banner@stapp03:/var/log/httpd/access_log /usr/src/finance/
sshpass -p 'BANNER_PASSWORD' scp -o StrictHostKeyChecking=no banner@stapp03:/var/log/httpd/error_log /usr/src/finance/
```

Replace BANNER_PASSWORD with the actual password.

## 8. Save and Test the Job

- Click Save.
- Click Build Now.

Check Console Output for success.

## 9. Verify Logs on Storage Server
```
ssh natasha@ststor01
ls -l /usr/src/finance
```

**Files expected:**

access_log
error_log

---

## Screenshots


<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/a74426c2-1cdc-485d-844a-3ac25a8ac214" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/777ff4dd-24f7-46e5-8430-379395092c1b" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/1ab7d8e8-0a93-4c94-bf87-b80b30562ba4" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/7b618e41-c3fc-4c85-b681-9a563731f292" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/e7cb679e-4615-406e-a3e2-ee10f93e2869" />


---



