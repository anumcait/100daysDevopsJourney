# 📘 Day 84 – Copy Data to App Servers using Ansible

KodeKloud 100 Days of DevOps Challenge
---

This task involves copying a file from the jump host to all application servers in Stratos DC using Ansible.

**You will create:**

- An inventory file
- An Ansible playbook

The validation system will run:
```
ansible-playbook -i inventory playbook.yml
```

So ensure your files work without extra arguments.

### ✅ Step 1: Create the Ansible Directory
```
mkdir -p /home/thor/ansible
cd /home/thor/ansible
```
### ✅ Step 2: Create the Inventory File

Create a file named inventory:
```
vi /home/thor/ansible/inventory
```

Add the application servers under a group called app.

Typical KodeKloud setup:
```
[app]
stapp01 ansible_user=tony ansible_ssh_pass=Sp3c!@l
stapp02 ansible_user=steve ansible_ssh_pass=Sp3c!@l
stapp03 ansible_user=bob ansible_ssh_pass=Sp3c!@l
```

⚠️ Use the actual credentials provided in your lab environment.

### ✅ Step 3: Create the Playbook

Create the playbook:
```
vi /home/thor/ansible/playbook.yml
```

Add the following:
```
---
- name: Copy itadmin index file to app servers
  hosts: app
  become: yes

  tasks:
    - name: Copy index.html to /opt/itadmin
      copy:
        src: /usr/src/itadmin/index.html
        dest: /opt/itadmin/index.html
```

**Explanation:**

hosts: app → Applies to all servers in the app group

become: yes → Required because /opt needs elevated privileges

copy module → Copies the file from controller to remote nodes

### ✅ Step 4: Validate (Run the Playbook)

In the Ansible directory:
```
cd /home/thor/ansible
ansible-playbook -i inventory playbook.yml
```

**Expected result:**

File /usr/src/itadmin/index.html should be copied to

/opt/itadmin/index.html on all application servers.
```
📂 Final Directory Structure
/home/thor/ansible/
├── inventory
└── playbook.yml
```
🎉 Task Completed!

You have successfully created:

- ✔ An Ansible inventory
- ✔ A working playbook
- ✔ A file copy operation across all application servers

---
