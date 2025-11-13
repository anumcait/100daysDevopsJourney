# Day 68: Set Up Jenkins Server

The DevOps team at xFusionCorp Industries is initiating the setup of CI/CD pipelines and has decided to utilize Jenkins as their server. Execute the task according to the provided requirements:


1. Install Jenkins on the jenkins server using the yum utility only, and start its service.
•	If you face a timeout issue while starting the Jenkins service, refer to this.
2. Jenkin's admin user name should be theadmin, password should be Adm!n321, full name should be Ammar and email should be ammar@jenkins.stratos.xfusioncorp.com.

Note:
1. To access the jenkins server, connect from the jump host using the root user with the password S3curePass.
2. After Jenkins server installation, click the Jenkins button on the top bar to access the Jenkins UI and follow on-screen instructions to create an admin user.

---

## Objective
Set up a Jenkins CI/CD server with proper configuration and fix common startup issues.

---

## Step 1: Connect to Jenkins Server

```bash
ssh root@<jenkins-server-ip>
# Password: S3curePass
```
Step 2: Install Jenkins
```bash
yum install -y jenkins
```
Check the installed .war file:
```bash
rpm -ql jenkins | grep jenkins.war
# Output: /usr/share/java/jenkins.war
```

## Step 3: Install Java 17 (Required for Jenkins 2.528.1+)
```
yum install -y java-17-openjdk java-17-openjdk-devel
alternatives --config java  # Select Java 17
java -version
Step 4: Fix Jenkins Service File
Edit systemd service:
```

vi /lib/systemd/system/jenkins.service
Update [Service] section:
```ini
[Service]
User=jenkins
Group=jenkins
ExecStart=/usr/bin/java -Djava.awt.headless=true -DJENKINS_HOME=/var/lib/jenkins -jar /usr/share/java/jenkins.war --httpPort=8080
TimeoutStartSec=300
Restart=on-failure
```
Save and exit.

## Step 5: Create Log Directory & Fix Permissions
```bash
mkdir -p /var/log/jenkins
chown -R jenkins:jenkins /var/log/jenkins /var/lib/jenkins
chmod 755 /var/log/jenkins /var/lib/jenkins
```
## Step 6: Reload Systemd and Start Jenkins
```bash
systemctl daemon-reload
systemctl enable jenkins
systemctl start jenkins
systemctl status jenkins -l
```
## Step 7: Access Jenkins UI
Open browser and go to:
```arduino
http://jenkins.stratos.xfusioncorp.com:8080
Follow on-screen instructions and create admin user:
Username: theadmin
Password: Adm!n321
Full Name: Javed
Email: javed@jenkins.stratos.xfusioncorp.com
```
## Troubleshooting
Service fails to start:
- Check Java version (java -version) → must be 17 or higher.
- Check log directory exists /var/log/jenkins and is owned by jenkins.
- Jenkins .war file not found:
- Correct path is /usr/share/java/jenkins.war (not /usr/lib/jenkins/jenkins.war).

## Permissions issues:

```bash
chown -R jenkins:jenkins /var/lib/jenkins /var/log/jenkins
chmod 755 /var/lib/jenkins /var/log/jenkins
```
## Key Takeaways
- Jenkins requires Java 17+ to run on newer versions.
- Correct directory paths and permissions are critical.
- Systemd service must point to the actual .war file.
- Always validate logs in /var/log/jenkins if startup fails.
