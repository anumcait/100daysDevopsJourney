# Day 82: Create Ansible Inventory for App Server Testing

## Objective
The Nautilus DevOps team is testing Ansible playbooks on various servers. For this task, we need to create an **INI-style inventory file** for **App Server 2 (stapp02)** so playbooks can run from the jump host without extra arguments.

---

## Steps

### 1️⃣ Navigate to the playbook directory
```bash
thor@jumphost ~$ cd /home/thor/playbook/
thor@jumphost ~/playbook$ ls
ansible.cfg  playbook.yml
```

---

### 2️⃣ Create the inventory file

Create a new file named inventory in the same directory:
```bash
thor@jumphost ~/playbook$ vi inventory
```
---

### 3️⃣ Add App Server 2 details

Inside inventory, add the following content:
```ini
[appservers]
stapp02 ansible_host=172.16.238.11 ansible_user=steve ansible_ssh_pass=Am3ric@

[all:vars]
ansible_python_interpreter=/usr/bin/python3
```

**Explanation:**

- stapp02 → Hostname of App Server 2.
- ansible_host → IP address of the server.
- ansible_user → SSH username.
- ansible_ssh_pass → SSH password.
- ansible_python_interpreter → Ensures Ansible uses Python 3 on the target.

Save and exit the editor (:wq in vi).

---

### 4️⃣ Test the inventory with the playbook

Run the playbook using the inventory file:
```bash
thor@jumphost ~/playbook$ ansible-playbook -i inventory playbook.yml
```
---
### 5️⃣ Expected output

```markdown
PLAY [all] ********************************************************************************************

TASK [Gathering Facts] ********************************************************************************
ok: [stapp02]

TASK [Install httpd package] **************************************************************************
changed: [stapp02]

TASK [Start service httpd] ****************************************************************************
changed: [stapp02]

PLAY RECAP ********************************************************************************************
stapp02                    : ok=3    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

```
✅ This confirms:

- Connection to stapp02 is successful.
- Playbook tasks executed correctly.
- No extra arguments required.

---

### screenshots

