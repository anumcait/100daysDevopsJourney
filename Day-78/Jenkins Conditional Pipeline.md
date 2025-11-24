# Day 78: Jenkins Conditional Pipeline
```
The development team of xFusionCorp Industries is working on to develop a new static website and they are planning to deploy the same on Nautilus App Servers using Jenkins pipeline. They have shared their requirements with the DevOps team and accordingly we need to create a Jenkins pipeline job. Please find below more details about the task:

Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and password Adm!n321.

Similarly, click on the Gitea button on the top bar to access the Gitea UI. Login using username sarah and password Sarah_pass123. There under user sarah you will find a repository named web_app that is already cloned on Storage server under /var/www/html. sarah is a developer who is working on this repository.

Add a slave node named Storage Server. It should be labeled as ststor01 and its remote root directory should be /var/www/html.

We have already cloned repository on Storage Server under /var/www/html.

Apache is already installed on all app Servers its running on port 8080.

Create a Jenkins pipeline job named xfusion-webapp-job (it must not be a Multibranch pipeline) and configure it to:

Add a string parameter named BRANCH.

It should conditionally deploy the code from web_app repository under /var/www/html on Storage Server, as this location is already mounted to the document root /var/www/html of app servers. The pipeline should have a single stage named Deploy ( which is case sensitive ) to accomplish the deployment.

The pipeline should be conditional, if the value master is passed to the BRANCH parameter then it must deploy the master branch, on the other hand if the value feature is passed to the BRANCH parameter then it must deploy the feature branch.

LB server is already configured. You should be able to see the latest changes you made by clicking on the App button. Please make sure the required content is loading on the main URL https://<LBR-URL> i.e there should not be a sub-directory like https://<LBR-URL>/web_app etc.

Note:
You might need to install some plugins and restart Jenkins service. So, we recommend clicking on Restart Jenkins when installation is complete and no jobs are running on plugin installation/update page i.e update centre. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case, please make sure to refresh the UI page.


For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.

```

---

# xFusionCorp Web App Deployment using Jenkins Pipeline

This document outlines the step-by-step process to deploy the `web_app` repository to Nautilus App Servers using a Jenkins conditional pipeline.

---

## 1. Prerequisites

- Jenkins installed and running.
- Gitea repository `web_app` cloned on Storage Server at `/var/www/html`.
- Apache installed and running on port 8080 on all app servers.
- LB server configured and accessible.
- Slave node (Storage Server) configured with label `ststor01`.

---

## 2. Install Java 17 (Required for Jenkins)

### CentOS Stream 9

```bash
sudo dnf install java-17-openjdk-devel -y
java -version
```

Verify output:

openjdk version "17.0.x" ...

---

## 3. Configure Jenkins Slave Node

Login to Jenkins: http://<JENKINS_URL>
Username: admin
Password: Adm!n321

Navigate: Manage Jenkins → Manage Nodes and Clouds → New Node

Configure:

Name: Storage Server

Type: Permanent Agent

Remote root directory: /var/www/html

Labels: ststor01

Launch method: Launch agents via SSH (provide credentials)

Save configuration.

---

## 4. Create Jenkins Pipeline Job

Navigate: New Item → Pipeline

Name: xfusion-webapp-job

Select Pipeline, not Multibranch.

Enable This project is parameterized:

Add String Parameter:

Name: BRANCH

Default value: master

---

## 5. Jenkins Pipeline Script

```
pipeline {
    agent { label 'ststor01' }
    
    parameters {
        string(name: 'BRANCH', defaultValue: 'master', description: 'Branch to deploy')
    }
    
    stages {
        stage('Deploy') {
            steps {
                script {
                    if (params.BRANCH == 'master' || params.BRANCH == 'feature') {
                        echo "Deploying ${params.BRANCH} branch..."
                        sh """
                        cd /var/www/html
                        git fetch --all
                        git reset --hard
                        git checkout ${params.BRANCH}
                        git pull origin ${params.BRANCH}
                        """
                    } else {
                        error "Invalid branch: ${params.BRANCH}. Use 'master' or 'feature'."
                    }
                }
            }
        }
    }
    
    post {
        success {
            echo "Deployment completed successfully!"
        }
        failure {
            echo "Deployment failed."
        }
    }
}

```
**Notes:**

git reset --hard ensures local changes do not block the branch switch.

Deployment occurs directly in /var/www/html so the LB serves content from the main URL without subdirectories.

---

## 6. Run the Job

Go to Jenkins: xfusion-webapp-job → Build with Parameters

Select branch: master or feature

Click Build

---

## 7. Verify Deployment

Open the Load Balancer URL: https://<LB-URL>

Confirm the website loads from /var/www/html directly, no /web_app subdirectory.

---

## Screenshots:

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/31e6ed68-b37b-40e4-ad22-3c45e11001f5" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/1badb82e-d01b-4929-a581-906bb166a356" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/4ee2ae98-3bf0-4a79-9ef3-da5a2c3631cb" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/1039d08e-0dbe-416f-af85-f62daddec2a4" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/50ae2bd6-e80f-4bf9-933e-ee016850729b" />








