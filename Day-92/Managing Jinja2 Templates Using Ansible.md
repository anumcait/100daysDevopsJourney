# Day 92 – Managing Jinja2 Templates Using Ansible  
KodeKloud 100 Days of DevOps Challenge

This task involves completing an HTTPD role by adding a Jinja2 template and updating the playbook to deploy it on **App Server 3 (stapp03)**.

---

## 📝 Requirements

### **a. Update `~/ansible/playbook.yml` to run the httpd role on App Server 3**

```yaml
---
- hosts: stapp03
  become: yes
  roles:
    - role/httpd
b. Create Jinja2 Template

File Path:
```swift
/home/thor/ansible/role/httpd/templates/index.html.j2
```

Content:
```jinja2
This file was created using Ansible on {{ inventory_hostname }}
```
Uses inventory_hostname so the file dynamically includes the correct server name.

---

c. Add Template Deployment Task

Update:

/home/thor/ansible/role/httpd/tasks/main.yml


Add:
```yaml
- name: Deploy index.html template
  template:
    src: index.html.j2
    dest: /var/www/html/index.html
    owner: "{{ ansible_user }}"
    group: "{{ ansible_user }}"
    mode: '0755'
```
- Deploys the template to /var/www/html/index.html
- Sets correct permissions (0755)
- Uses the respective sudo user as owner/group via ansible_user

▶️ Run the Playbook
```
ansible-playbook -i inventory playbook.yml
```
Screenshots:
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/ba11c2b8-dc3b-4780-b385-489606348c79" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/f9301214-0f7f-4d3b-ba00-e800a24aa78e" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/a30142e8-bdb8-4c07-b9ce-34452db80fab" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/0b5fd83d-1d03-46f1-a66c-204f3dacef52" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/702c837b-7269-405f-b0af-2e63d6c35a69" />







