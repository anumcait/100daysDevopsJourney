# Day 91 – Ansible Lineinfile Module

## Task: Install httpd & Manage index.html on All App Servers

---

## Step-by-Step Instructions

### 1. Go to the Ansible directory
```
cd /home/thor/ansible
```

---

### 2. Verify that the inventory file exists
```bash
ls -l inventory
```

---

### 3. Create the playbook
Create a file named `playbook.yml`:

```bash
vi playbook.yml
```


Paste the following content:
```bash
---
- name: Install and configure httpd with sample page
  hosts: app_servers
  become: yes

  tasks:

    - name: Install httpd package
      yum:
        name: httpd
        state: present
        update_cache: yes

    - name: Ensure httpd service is running and enabled
      service:
        name: httpd
        state: started
        enabled: yes

    - name: Create index.html with base content
      copy:
        dest: /var/www/html/index.html
        content: "This is a Nautilus sample file, created using Ansible!\n"

    - name: Add line at top using lineinfile
      lineinfile:
        path: /var/www/html/index.html
        line: "Welcome to xFusionCorp Industries!"
        insertbefore: BOF

    - name: Set ownership of index.html
      file:
        path: /var/www/html/index.html
        owner: apache
        group: apache

    - name: Set permissions of index.html
      file:
        path: /var/www/html/index.html
        mode: '0644'
```

Save & exit:

---

### 4. Run the playbook

ansible-playbook -i inventory playbook.yml

---

Tasks Completed Successfully 🎉

### Screenshots:

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/98eb09d3-03ef-4c0a-8841-70c146635981" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/f0f8aeeb-12fc-4f89-a249-6bf931197800" />




