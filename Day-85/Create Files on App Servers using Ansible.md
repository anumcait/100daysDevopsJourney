# Day 85: Create Files on App Servers Using Ansible

## Objective

In this exercise, we will use **Ansible** to automate the following tasks:

- Create a file `/home/web.txt` on multiple app servers.
- Set the file permissions to `0777`.
- Set different user/group ownership for each app server.

## Prerequisites

Before you begin, ensure that you have the following:

- **Ansible** installed on the **jump host**.
- SSH access to all **app servers** (`stapp01`, `stapp02`, `stapp03`).
- Sudo privileges on all servers to modify file permissions and ownership.

## Directory Structure

Ensure that the directory structure looks like this:
```
~/playbook/
├── inventory
└── playbook.yml
```

## Step-by-Step Instructions

### 1. Create the Inventory File

Create the **inventory file** in the `~/playbook/inventory` directory. This inventory file will list all app servers and assign each one a `file_owner` variable.

Run the following command to create the file:

```bash
nano ~/playbook/inventory
```
Add the following content to the file:
```
[appservers]
stapp01 ansible_host=stapp01 ansible_ssh_pass=<<Password>> file_owner=tony
stapp02 ansible_host=stapp02 ansible_ssh_pass=<<Password>> file_owner=steve
stapp03 ansible_host=stapp03 ansible_ssh_pass=<<Password>> file_owner=banner
```
---

### 2. Create the Playbook File

Create the playbook.yml file in the ~/playbook directory. This playbook will handle the following tasks:

Create the file /home/web.txt on all app servers.

Set the correct file permissions (0777).

Set ownership for the file based on the app server.

Run the following command to create the playbook file:

nano ~/playbook/playbook.yml


Add the following content to the file:
```
---
- name: Create /home/web.txt on all app servers
  hosts: appservers
  become: yes
  tasks:
    - name: Create file with correct ownership and permissions
      file:
        path: /home/web.txt
        state: touch
        mode: '0777'
        owner: "{{ file_owner }}"
        group: "{{ file_owner }}"
```

---

### 3. Run the Playbook

Once you have both the inventory and playbook.yml files set up, you can run the Ansible playbook.

Navigate to the ~/playbook/ directory and run the following command:
```
ansible-playbook -i inventory playbook.yml
```
Expected Output

Ansible will execute the playbook and create the /home/web.txt file on all app servers. You should see output similar to the following:
```
PLAY [Create /home/web.txt on all app servers] ***

TASK [Create file with correct ownership and permissions] ***
changed: [stapp01] => (item=None) => {
    "changed": true,
    "path": "/home/web.txt",
    "mode": "0777",
    "owner": "tony",
    "group": "tony"
}
...
```
---

### 4. Verify the Changes

After running the playbook, verify the following on each app server:

File existence: The file /home/web.txt should exist.

Permissions: The file should have permissions 0777.

Ownership: The file should have the correct owner and group, as follows:

stapp01: Owner is tony

stapp02: Owner is steve

stapp03: Owner is banner

You can verify these changes by running the following command on each server:

```
ls -l /home/web.txt
```
---

### Troubleshooting

If you encounter issues, here are some common problems and solutions:

Undefined variables error:
If you see an error like file_owner is undefined, check your inventory file for correct formatting. The file_owner variable should be defined on the same line as the host.

Permission issues:
If you encounter permission errors, ensure that the user running the Ansible playbook has sudo privileges on all app servers.

SSH Connectivity:
Ensure SSH access is properly configured, and the servers are reachable from the jump host.

---

### Screenshots

<img width="500" height="300" alt="Screenshot 2025-11-30 201000" src="https://github.com/user-attachments/assets/ca576c31-8ce3-4995-8b7d-e29c9efcb429" />

<img width="500" height="300" alt="Screenshot 2025-11-30 201050" src="https://github.com/user-attachments/assets/47aeec0b-4a33-4dfb-87e0-092cd8d7b0f9" />

<img width="500" height="300" alt="Screenshot 2025-11-30 201116" src="https://github.com/user-attachments/assets/20421c3b-d9d7-4845-9a4e-fc7171c80866" />





