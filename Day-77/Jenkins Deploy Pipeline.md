# Day 77: Jenkins Deploy Pipeline

## 1. Overview
This document describes the steps to set up a Jenkins pipeline for deploying the static website repository `web_app` to Nautilus App Servers. The deployment uses a Jenkins slave node named **Storage Server** and deploys to `/var/www/html`.

---

## 2. Prerequisites
- Jenkins server installed and running.
- Gitea repository `web_app` accessible.
- Apache installed and running on all App Servers on port 8080.
- Storage Server accessible via SSH from Jenkins.
- Jenkins user: `admin`  
- Gitea user: `sarah`  

---

## 3. Jenkins Slave Node Setup

1. Log in to Jenkins UI as `admin`.
2. Go to **Manage Jenkins → Manage Nodes and Clouds → New Node**.
3. Configure the slave node:
   - **Node Name:** Storage Server
   - **Remote root directory:** `/var/www/html`
   - **Labels:** `ststor01`
   - **Launch method:** Launch agents via SSH
   - **Host:** IP or hostname of Storage Server
   - **Credentials:** Add user `natasha` or the user with access
4. Test connection. The node should come online successfully.

---

## 4. Repository Setup

1. Log in to Gitea as `sarah`.
2. Verify the repository `web_app` exists.
3. Ensure repository is cloned on Storage Server:

```bash
cd /var/www/html
ls -la
```

You should see:

.git/
index.html

---

## 4. Ensure proper ownership and permissions:
```
sudo chown -R natasha:natasha /var/www/html
sudo chmod -R 755 /var/www/html
```
---

## 5. Jenkins Pipeline Job

In Jenkins, click New Item → Pipeline → OK.

Name the job: datacenter-webapp-job.

Under Pipeline section, configure:
```
pipeline {
    agent { label 'ststor01' }
    stages {
        stage('Deploy') {
            steps {
                dir('/var/www/html/web_app') {
                    // Clone or update repository
                    script {
                        if (!fileExists('.git')) {
                            sh 'git clone http://git.stratos.xfusioncorp.com/sarah/web_app.git .'
                        } else {
                            sh 'git pull'
                        }
                    }
                }
                // Copy files to document root
                sh 'cp -r /var/www/html/web_app/. /var/www/html/'
            }
        }
    }
}
```

Save the pipeline.

## 6. Running the Pipeline
```
Click Build Now.

Monitor the console output.

Ensure the stage Deploy completes without errors.
```
- Confirm files in /var/www/html:

ls -la /var/www/html

---

## 7. Verification

Open a browser and access the LB URL:

https://<LBR-URL>


Ensure the website loads correctly.

There should be no /web_app subdirectory in the URL.

---

## 8. Troubleshooting

Agent Launch Errors:

Ensure /var/www/html exists and user has write permission.

Correct ownership: sudo chown -R natasha:natasha /var/www/html

Git clone errors:

Check network access to Gitea.

Use the correct repository URL: http://git.stratos.xfusioncorp.com/sarah/web_app.git

Pipeline permission denied errors:

Check permissions for all files in /var/www/html.

Ensure the Jenkins agent user matches the file owner.

---

## 9. Notes

Always restart Jenkins after installing plugins.

Use dir() step in pipeline to handle specific directories.

Keep Jenkins pipeline idempotent (able to run multiple times without breaking).

The Deploy stage name is case sensitive.

✔ HOW TO FIX IT (100% Working Solutions) : For Plugins installation
✅  Switch update site to HTTPS mirror
```
Jenkins → Manage Jenkins

Manage Plugins

Advanced

Find Update Site URL

REPLACE default:

https://updates.jenkins.io/update-center.json


WITH:

🔥 Mirror 1 (most reliable in labs)

https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json


🔥 Mirror 2

https://mirrors.cloud.tencent.com/jenkins/updates/update-center.json


Click Submit

Restart Jenkins
```


