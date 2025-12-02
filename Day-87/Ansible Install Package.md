# 📘 Day 87 — Ansible Install Package

This task involves installing the httpd package on all application servers inside the Stratos Datacenter using Ansible from the jump host.

---

### ✅ Step 1 — Create Playbook Directory
```
mkdir -p /home/thor/playbook
```

### ✅ Step 2 — Create the Inventory File

Path:
```
/home/thor/playbook/inventory
```

Add the following (KodeKloud default credentials):
```
[apps]
stapp01 ansible_host=172.16.238.10 ansible_user=tony   ansible_ssh_pass=Ir0nM@n  ansible_become_pass=Ir0nM@n
stapp02 ansible_host=172.16.238.11 ansible_user=steve  ansible_ssh_pass=Am3ric@  ansible_become_pass=Am3ric@
stapp03 ansible_host=172.16.238.12 ansible_user=banner ansible_ssh_pass=BigGr33n ansible_become_pass=BigGr33n
```

⚠️ Passwords must not be quoted.
⚠️ Ensure “0” in Ir0nM@n is a zero.

### ✅ Step 3 — Create the Ansible Playbook

Path:
```
/home/thor/playbook/playbook.yml
```

Contents:
```
---
- name: Install httpd on all app servers
  hosts: apps
  become: yes
  tasks:
    - name: Install httpd package
      yum:
        name: httpd
        state: present
```
### ✅ Step 4 — Ensure Correct Permissions

sudo chown -R thor:thor /home/thor/playbook

### ✅ Step 5 — Test SSH Access (Optional)

You can manually validate login to each server:
```
ssh tony@172.16.238.10
ssh steve@172.16.238.11
ssh banner@172.16.238.12
```

### ✅ Step 6 — Run the Playbook

From inside the directory:
```
cd /home/thor/playbook
ansible-playbook -i inventory playbook.yml
```

Expected output:

- All servers reachable
- All tasks executed successfully
- httpd installed on all app servers

### 🧹 Troubleshooting

🔸 Permission denied (publickey,password)

Password incorrect in inventory → recheck values.

🔸 MODULE FAILURE / rc=137

Most likely wrong sudo password → correct ansible_become_pass.


### 🎉 Task Completed

You have successfully:

- ✔️ Created an inventory
- ✔️ Created a playbook
- ✔️ Installed packages on remote servers via Ansible

### Screenshots

<img width="1050" height="531" alt="image" src="https://github.com/user-attachments/assets/2247dfd1-cbf7-4602-ad69-4ae6f7462edf" />

<img width="1050" height="522" alt="image" src="https://github.com/user-attachments/assets/337bc8b8-ced9-4a14-bbaf-f8fd9f6bad1c" />



