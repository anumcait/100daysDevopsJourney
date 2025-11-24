The development team of xFusionCorp Industries is working on to develop a new static website and they are planning to deploy the same on Nautilus App Servers using Jenkins pipeline. They have shared their requirements with the DevOps team and accordingly we need to create a Jenkins pipeline job. Please find below more details about the task:



Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and password Adm!n321.


Similarly, click on the Gitea button on the top bar to access the Gitea UI. Login using username sarah and password Sarah_pass123. There under user sarah you will find a repository named web_app that is already cloned on Storage server under /var/www/html. sarah is a developer who is working on this repository.


Add a slave node named Storage Server. It should be labeled as ststor01 and its remote root directory should be /var/www/html.


We have already cloned repository on Storage Server under /var/www/html.


Apache is already installed on all app Servers its running on port 8080.


Create a Jenkins pipeline job named xfusion-webapp-job (it must not be a Multibranch pipeline) and configure it to:


Deploy the code from web_app repository under /var/www/html on Storage Server, as this location is already mounted to the document root /var/www/html of app servers. The pipeline should have a single stage named Deploy ( which is case sensitive ) to accomplish the deployment.

LB server is already configured. You should be able to see the latest changes you made by clicking on the App button. Please make sure the required content is loading on the main URL https://<LBR-URL> i.e there should not be a sub-directory like https://<LBR-URL>/web_app etc.


Note:


You might need to install some plugins and restart Jenkins service. So, we recommend clicking on Restart Jenkins when installation is complete and no jobs are running on plugin installation/update page i.e update centre. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case, please make sure to refresh the UI page.


For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.


====

# xFusionCorp Static Website Deployment Using Jenkins Pipeline

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


