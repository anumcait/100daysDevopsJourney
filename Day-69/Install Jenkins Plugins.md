# Day 69: Install Jenkins Plugins : Git and GitLab

The Nautilus DevOps team has recently setup a Jenkins server, which they want to use for some CI/CD jobs. Before that they want to install some plugins which will be used in most of the jobs. Please find below more details about the task

1. Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and Adm!n321 password.

2. Once logged in, install the Git and GitLab plugins. Note that you may need to restart Jenkins service to complete the plugins installation, If required, opt to Restart Jenkins when installation is complete and no jobs are running on plugin installation/update page i.e update centre.

Note:

1. After restarting the Jenkins service, wait for the Jenkins login page to reappear before proceeding.

2. For tasks involving web UI changes, capture screenshots to share for review or consider using screen recording software like loom.com for documentation and sharing.

---

## 1. Access Jenkins Dashboard

1. Open your browser and navigate to your Jenkins instance (e.g., http://<jenkins-server>:8080).
2. Log in using the username admin and password Adm!n321.

## 2. Install Git Plugin

The Git plugin is required to integrate Jenkins with Git repositories (e.g., GitHub).

1. Go to Manage Jenkins > Manage Plugins.
2. Click on the Available tab.
3. In the Filter box, type git.
4. Check the checkbox for the Git plugin.
5. Click on Install without Restart.
6. Wait for the installation to complete.

## 3. Install GitLab Plugin

The GitLab plugin integrates Jenkins with GitLab repositories for Continuous Integration.

1. Go to Manage Jenkins > Manage Plugins.
2. Click on the Available tab.
3. In the Filter box, type gitlab.
4. Check the checkbox for the GitLab Plugin.
5. Click on Install without Restart.
6. Wait for the installation to complete.

If you receive an error about missing dependencies (e.g., Jackson2 API plugin), follow the steps below.

## 4. Resolve Jackson2 API Plugin Dependency Issue

Sometimes Jenkins fails to download necessary dependencies automatically, such as the Jackson2 API plugin.

Steps to Manually Install Jackson2 API Plugin:

Download the Jackson2 API plugin from the Jenkins update center:
Download jackson2-api 2.20.1
Upload the plugin manually:
Go to Manage Jenkins > Manage Plugins > Advanced Tab.
Scroll to the Upload Plugin section.
Click on Choose File and select the downloaded jackson2-api.hpi file.
Click Upload.
Restart Jenkins:

Go to Manage Jenkins > Restart Jenkins.
1. Wait for Jenkins to restart.
2. Verifying Jackson2 API Installation:
    - After Jenkins restarts, go to Manage Jenkins > Manage Plugins > Installed tab.
3. Verify that the Jackson2 API plugin is now listed.

## 5. Install GitLab Plugin Manually (If Needed)

If the GitLab plugin still hasn’t installed correctly, follow these steps:

- Download the GitLab plugin manually from the Jenkins Plugin Repository:
- Download GitLab Plugin (1.9.8)
- Upload the plugin manually:
- Go to Manage Jenkins > Manage Plugins > Advanced Tab.
- Under Upload Plugin, click on Choose File and select the downloaded gitlab-plugin.hpi.
- Click Upload.

Restart Jenkins again after uploading the plugin.

## 6. Verify Plugin Installation

After restarting Jenkins, verify that both the Git and GitLab plugins are installed correctly:

- Go to Manage Jenkins > Manage Plugins > Installed tab.
- Verify that Git Plugin and GitLab Plugin are listed as installed.
- If both plugins are present and there are no errors, you're all set!


## 7. Troubleshooting

If you encounter any issues during installation, consider the following:

Network Issues: Ensure that your Jenkins server has internet access, especially if you're behind a firewall or proxy. If needed, configure the proxy settings under Manage Jenkins > Manage Plugins > Advanced.

Plugin Compatibility: Ensure the versions of the plugins you're using are compatible with your Jenkins version. Older versions of plugins might require you to install compatible versions.

Log Files: Check the Jenkins log files (/var/log/jenkins/jenkins.log) for more detailed error messages that might help you identify the issue.

Example of a Fully Installed and Configured Jenkins Job
Jenkins → Manage Jenkins → Manage Plugins → Installed:
  - Git Plugin: Version 4.12.3
  - GitLab Plugin: Version 1.9.8
  - Jackson2 API Plugin: Version 2.20.1

<img width="1050" height="488" alt="image" src="https://github.com/user-attachments/assets/509cc11b-2d0d-4131-9c3f-b1da0c461ae4" />

<img width="1049" height="516" alt="image" src="https://github.com/user-attachments/assets/85bf9a2e-9009-49c4-aa0e-ddeca060ce88" />



