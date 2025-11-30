# Day 83 – Troubleshoot & Create Ansible Playbook
_KodeKloud 100 Days of DevOps Challenge_

---

## 📝 Task Summary
A team member left an incomplete Ansible setup. Your job is to:

1. Fix the inventory file so the playbook targets **App Server 1 (stapp01)**
2. Create a playbook to generate an empty file `/tmp/file.txt`
3. Ensure the playbook runs using:

---

ansible-playbook -i inventory playbook.yml


---

### 🔧 Step 1 — Fix the Inventory File

Edit the inventory file:
```
/home/thor/ansible/inventory
```

Replace everything with:

```ini
[appservers]
stapp01 ansible_host=172.16.238.10 ansible_user=tony ansible_ssh_pass=Ir0nM@n ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```
---
### 🔧 Step 2 — Create the Playbook

Create the playbook file:
```
/home/thor/ansible/playbook.yml
```

Add the following content:

---
```
- name: Create file on App Server 1
  hosts: appservers
  become: yes

  tasks:
    - name: Create empty file in /tmp
      file:
        path: /tmp/file.txt
        state: touch
```
---

## ▶️ Step 3 — Run and Validate

**Run:**
```
ansible-playbook -i inventory playbook.yml
```

```
Expected output:

PLAY [Create file on App Server 1] *********************************************

TASK [Gathering Facts] *********************************************************
ok: [stapp01]

TASK [Create empty file in /tmp] ***********************************************
changed: [stapp01]

PLAY RECAP *********************************************************************
stapp01  : ok=2  changed=1  unreachable=0  failed=0
```

### 📁 Final Directory Structure
```
ansible/
├── inventory
└── playbook.yml
```
### ✅ Result

- You have successfully:
- Fixed the Ansible inventory
- Created a functional Ansible playbook
- Executed it on App Server 1

### Screenshots

<img width="700" height="500" alt="Screenshot 2025-11-30 105727" src="https://github.com/user-attachments/assets/81433ec6-829e-4a3a-b618-c2a5dcbb1868" />
