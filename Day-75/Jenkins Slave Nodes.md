# Day 75: Jenkins Slave Nodes

## Objective

Add all application servers as Jenkins slave nodes (agents) for CI/CD and automation tasks.

---

## Prerequisites

- Jenkins server installed and running
- Application servers available
- SSH access to app servers
- Java installed on Jenkins master

---

## Step 1: Generate SSH Key for Jenkins

On the **Jenkins server**:

```bash
# Switch to Jenkins user
sudo su - jenkins

# Create SSH key for Jenkins
ssh-keygen -t rsa -b 2048 -f ~/.ssh/jenkins_key
# Press Enter for all prompts (leave passphrase empty)
```
## Step 2: Copy Public Key to App Servers
```bash
ssh-copy-id -i ~/.ssh/jenkins_key.pub tony@stapp01
ssh-copy-id -i ~/.ssh/jenkins_key.pub steve@stapp02
ssh-copy-id -i ~/.ssh/jenkins_key.pub banner@stapp03
```
- Enter the respective user password when prompted
- Ensures passwordless SSH from Jenkins

## Step 3: Install Java on App Servers

Jenkins agents require Java to run the remoting JAR.

For RHEL/CentOS:
```bash
sudo yum install java-17-openjdk -y
```
For Ubuntu/Debian:
```bash
sudo apt install openjdk-17-jdk -y
```

Verify:
```bash
java -version
```

Expected output:
```bash
openjdk version "17.0.x" ...
```

| ⚠ Ensure Java 17 is the default version if multiple versions exist:

---

## Step 4: Add SSH Credentials in Jenkins

Open Jenkins UI → Manage Jenkins → Credentials → Global → Add Credentials

Kind: SSH Username with private key

Username: tony / steve / banner

Private Key: Enter directly → paste contents of ~/.ssh/jenkins_key

Passphrase: leave empty

Save

---

## Step 5: Add Jenkins Slave Nodes

Go to Manage Jenkins → Manage Nodes → New Node

Enter details:

|Field|	App_server_1|	App_server_2|	App_server_3|
|-----|-------------|-------------|-------------|
|Name|	App_server_1|	App_server_2|	App_server_3|
|Remote root directory|	/home/tony/jenkins	|/home/steve/jenkins	|/home/banner/jenkins|
|Labels|	stapp01|	stapp02|	stapp03|
|Number of executors|	1|	1|	1|
|Usage|	Use this node as much as possible|	Use this node as much as possible|	Use this node as much as possible|
|Launch method|	Launch agents via| SSH	Launch agents via |SSH	Launch agents via SSH|
|Host|	stapp01|	stapp02|	stapp03|
|Credentials|	Select the respective Jenkins SSH key credential|	Select credential|	Select credential|
|Host Key| Verification Strategy	|Known hosts file|	Known hosts file|	Known hosts file|

Click Save

---

## Step 6: Launch Agent and Verify

Go to the node → Click Launch agent

Check logs to ensure the agent connects successfully:
```
[SSH] Starting agent process...
Agent successfully connected
```

Nodes should appear online in Jenkins.

---

### Notes

- Jenkins cannot access private keys from other users’ home directories; always generate a dedicated key for Jenkins.
- Java 17 is required for Jenkins agents with recent Jenkins versions.
- Use labels to target nodes in your CI/CD jobs.

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/5adaae8b-848a-40a4-9170-b79785396d48" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/5ab6a66e-9ec1-4ced-90b4-7135894fd9ce" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/afd8efd7-1965-4b75-9398-053e6bc4de04" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/6b16ac0f-24ab-4a9b-a84f-1fca5111555f" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/285662b1-1f4c-4173-b1c9-a2557ae1c047" />





