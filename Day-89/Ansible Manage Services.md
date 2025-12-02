## Day 89 — Ansible Manage Services

### Task Summary

Developers require specific dependencies to be installed and managed on Nautilus app servers in Stratos DC. As part of DevOps automation, we must create an Ansible playbook that installs and manages services on all app servers.

This task includes:

Creating an Ansible playbook to install httpd

Ensuring the service is enabled and running

Using the existing inventory file

Allowing user thor to run the playbook without extra arguments

### ✅ Step-by-Step Procedure

#### 1. Switch to the jump host

You will be working on the jump host where Ansible is already installed.
```
ssh thor@jump_host
```
---

#### 2. Create the Ansible directory (if not already present)
```
mkdir -p /home/thor/ansible
```
---

#### 3. Verify the inventory file exists

The required inventory file is already provided:
```
ls -l /home/thor/ansible/inventory
```

Example expected structure of the inventory:
```
[appservers]
stapp01
stapp02
stapp03
```
---

#### 4. Create the Ansible playbook

Create the file:
```
nano /home/thor/anssible/playbook.yml
```

Add the following content:
```
---
- name: Install and configure httpd on app servers
  hosts: appservers
  become: yes

  tasks:
    - name: Install httpd package
      package:
        name: httpd
        state: present

    - name: Ensure httpd service is started and enabled
      service:
        name: httpd
        state: started
        enabled: yes
```

Save and exit (CTRL + O, Enter, CTRL + X).

---

#### 5. Ensure correct permissions

Make sure user thor can run the playbook:
```
sudo chown -R thor:thor /home/thor/ansible
```
---

#### 6. Test the playbook

Navigate to the Ansible directory:
```
cd /home/thor/ansible
```

Run the playbook exactly the same way the validation will run it:
```
ansible-playbook -i inventory playbook.yml
```
---

### 🎉 Result

If everything is correct:

httpd will be installed on all app servers

The service will be enabled and running

The playbook will run successfully without passing any extra arguments

Validation in the KodeKloud game will pass
