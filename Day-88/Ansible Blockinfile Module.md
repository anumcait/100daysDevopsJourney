# 🚀 Day 88 — Ansible `blockinfile` Module (KodeKloud 100 Days of DevOps)

This task sets up an **httpd web server** on all application servers in the Stratos DC using **Ansible**, and deploys a sample webpage using the **blockinfile** module.

---

## ✅ Requirements

1. Use the existing inventory file at:
```
/home/thor/ansible/inventory
```
2. Create a new playbook:
```
/home/thor/ansible/playbook.yml
```
3. Install and start **httpd** on all app servers.
4. Use **blockinfile** to add the following content to:
```
/var/www/html/index.html
```

Content:

```
Welcome to XfusionCorp!

This is Nautilus sample file, created using Ansible!

Please do not modify this file manually!

```

5. Set:
- owner: `apache`
- group: `apache`
- permissions: `0755`

6. **Do NOT** use custom or empty markers for the blockinfile module.

7. The playbook must work with:
```
ansible-playbook -i inventory playbook.yml
```
---

## 🗂️ Inventory File

Your given inventory contains:

stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n ansible_user=tony
stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@ ansible_user=steve
stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n ansible_user=banner

There is **no group defined**, so the playbook will target **all** hosts.

---

## 📘 Final `playbook.yml`

```yaml
---
- name: Setup httpd and deploy sample webpage
  hosts: all
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

    - name: Ensure index.html file exists
      file:
        path: /var/www/html/index.html
        state: touch

    - name: Add sample content to index.html using blockinfile
      blockinfile:
        path: /var/www/html/index.html
        block: |
          Welcome to XfusionCorp!
          
          This is  Nautilus sample file, created using Ansible!
          
          Please do not modify this file manually!

    - name: Set owner, group and permissions for index.html
      file:
        path: /var/www/html/index.html
        owner: apache
        group: apache
        mode: '0755'
```

---

## ▶️ Running the Playbook

Execute:
```
ansible-playbook -i inventory playbook.yml
```

**Expected result:**

- httpd installed
- httpd started and enabled
- /var/www/html/index.html created
- Content inserted via blockinfile (with default markers)
- File owned by apache:apache
- Permissions set to 0755

## Screenshots:

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/3389ad1d-fc27-48a5-9c18-1ed867debb29" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/c3db5b29-5b23-4030-a5ec-a2ecd2dd318e" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/ef5a5db6-7788-4844-b87e-35a4839abaa7" />

---




