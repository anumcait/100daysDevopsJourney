# Day 86 – Ansible Ping Module Usage  
_KodeKloud 100 Days of DevOps Challenge_

This task focuses on configuring **password-less SSH** between the Ansible controller (Jump Host) and a managed node, then using the **Ansible ping module** to verify connectivity.

---

## 🚀 Task Overview

- **Ansible Controller:** Jump Host  
- **User:** `thor`  
- **Inventory File:** `/home/thor/ansible/inventory`  
- **Goal:** Configure SSH key-based authentication and run Ansible `ping` against **stapp03**.

---

### 1️⃣ Switch to the `thor` User

```bash
sudo su - thor
```
---


### 2️⃣ Check the Existing Inventory File
```
cat /home/thor/ansible/inventory
```

Example content:
```
stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n ansible_user=tony
stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@ ansible_user=steve
stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n ansible_user=banner
```
---


### 3️⃣ Generate SSH Key Pair on the Controller
```
ssh-keygen -t rsa -N "" -f ~/.ssh/id_rsa
```

Press **Enter** to accept default values.

---

### 4️⃣ Copy SSH Public Key to App Server 3

The correct SSH username for stapp03 is banner.
```
ssh-copy-id banner@stapp03
```
You should see:

Number of key(s) added: 1

---

### 5️⃣ Test Password-less SSH Login
ssh banner@stapp03


If it logs in without a password → SSH setup is successful.

Exit back:
```
exit
```
---

### 6️⃣ Update the Ansible Inventory

Edit the inventory file:
```
vi /home/thor/ansible/inventory
```

Update the stapp03 entry:
```
stapp03 ansible_host=172.16.238.12 ansible_user=banner
```
---

### 7️⃣ Test Ansible Connectivity with Ping Module
ansible -i /home/thor/ansible/inventory stapp03 -m ping


Expected output:
```
stapp03 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

---

✅ Result

You have successfully:

- Generated SSH keys
- Configured password-less SSH to App Server 3
- Updated the Ansible inventory
- Verified connectivity using the ping module

Your Ansible controller is now correctly set up to run playbooks on stapp03.

---

### Screenshots:

<img width="1049" height="522" alt="image" src="https://github.com/user-attachments/assets/edffc796-fcce-4dc9-8545-8eaac8365b1a" />

---




