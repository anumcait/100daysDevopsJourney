# Day 81: Jenkins Multistage Pipeline – xFusionCorp Static Website Deployment

## 1. Prerequisites

- **Jenkins UI credentials:**  
  - Username: `admin`  
  - Password: `Adm!n321`  
- **Gitea UI credentials:**  
  - Username: `sarah`  
  - Password: `Sarah_pass123`  
- **Repository:** `sarah/web`  
- **Storage server repository path:** `/var/www/html`  
- **Apache installed** on all app servers (port 8080)  
- **Load balancer URL:** `http://stlb01:8091`  

---

## 2. Git Steps – Update Website Content

1. **Login to the Storage server:**

```bash
ssh natasha@ststor01
```
2. **Navigate to the repository:**
```bash
cd /var/www/html/web
```
3. **Check repository status:**
```bash
git status
```
4. **Update index.html**

```
nano index.html
```
Replace the content with:
```css
Welcome to xFusionCorp Industries
```
Save and exit (CTRL+O, ENTER, CTRL+X).

5. **Stage and commit changes:**
```
git add index.html
git commit -m "Update index.html with welcome message"
```

6. **Push changes to master branch:**
```bash
git push origin master
# Enter credentials:
# Username: sarah
# Password: Sarah_pass123
```
7. **Verify on Gitea:**

- Login → Navigate to sarah/web → Ensure latest commit shows updated index.html.

---

## 3. Jenkins Pipeline – Create deploy-job

- Login to Jenkins:

  URL: Jenkins UI → Username: admin, Password: Adm!n321

- Create a new pipeline job:
- Click New Item → Name: deploy-job → Select Pipeline → Click OK

3. **Pipeline Script:**
```groovy
pipeline {
    agent any

    environment {
        STORAGE_SERVER = "ststor01"
        STORAGE_USER = "natasha"
        STORAGE_PASS = "Natasha_pass123"
        REMOTE_DIR = "/var/www/html"
        LBR_URL = "http://stlb01:8091"
        REPO_URL = "http://git.stratos.xfusioncorp.com/sarah/web.git"
        BRANCH = "master"
    }

    stages {
        stage('Deploy') {
            steps {
                echo 'Deploying website to app servers...'

                // Clone repository in Jenkins workspace
                git url: "${REPO_URL}", branch: "${BRANCH}"

                // Ensure sshpass is installed
                sh 'which sshpass || sudo yum install -y sshpass || sudo apt-get install -y sshpass'

                // Copy website files to Storage server
                sh """
                    sshpass -p '${STORAGE_PASS}' scp -o StrictHostKeyChecking=no -r * ${STORAGE_USER}@${STORAGE_SERVER}:${REMOTE_DIR}/
                """
            }
        }

        stage('Test') {
            steps {
                echo 'Testing website accessibility...'

                // Test website via curl
                sh """
                    curl -f ${LBR_URL} || exit 1
                """
            }
        }
    }

    post {
        success {
            echo 'Deployment and test completed successfully!'
        }
        failure {
            echo 'Pipeline failed! Check logs for errors.'
        }
    }
}

```
---

## 4. Pipeline Explanation

- Deploy Stage:

    - Clones the latest repo from Gitea
    - Copies files to /var/www/html on Storage server via sshpass using natasha credentials

- Test Stage:

    - Uses curl to verify the website is accessible via the load balancer URL
    - Automatically fails if the website is not accessible

- Post Actions:
    - Displays success or failure message in Jenkins console

---

## 5. Verification

1. Click **App button** on Jenkins top bar or visit:
```
http://stlb01:8091
```

2. **Check content:**

The website should display:
```css
Welcome to xFusionCorp Industries
```
3. Ensure URL does not contain /web subdirectory.

---

## 6. Notes

- Jenkins plugins (Git, SSH, Pipeline) may need installation.
- Restart Jenkins if required via Manage Jenkins → Restart.
- For UI tasks, take screenshots or record your screen for verification.

---

## Screenshots

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/b3e3823c-0d12-4aa6-91dd-33822542b4e1" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/3e4f511d-f464-4c39-a198-1e67a4c7fa55" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/6941e406-7884-4741-b6a6-fdf7ad6b533f" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/aa2612e4-b118-4b55-8dbb-1fa76ba073fb" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/089e3da7-fa2b-4cc1-9838-586e38e83bcf" />








