# Day 90 — Managing ACLs Using Ansible  
KodeKloud – 100 Days of DevOps Challenge

---

## **Task Summary**

You must create ACL-controlled files on each app server using **Ansible only**.

- Playbook name: `playbook.yml`
- Location: `/home/thor/ansible/`
- Inventory file already exists in the same directory.
- Validation command:

ansible-playbook -i inventory playbook.yml


### **Requirements per Server**

| App Server | File Name     | ACL Entity | Entity Type | Permissions |
|-----------|---------------|------------|-------------|-------------|
| stapp01   | blog.txt      | tony       | group       | r           |
| stapp02   | story.txt     | steve      | user        | rw          |
| stapp03   | media.txt     | banner     | group       | rw          |

All files must be:
- Empty
- Located in `/opt/data/`
- Owned by `root`

---

## **✔ playbook.yml (Use this exact content)**

```yaml
---
- name: Manage ACLs on app servers
hosts: all
become: yes

tasks:

  - name: Create file blog.txt on app server 1
    file:
      path: /opt/data/blog.txt
      state: touch
      owner: root
      group: root
    when: inventory_hostname == "stapp01"

  - name: Set ACL for group tony on blog.txt
    acl:
      path: /opt/data/blog.txt
      entity: tony
      etype: group
      permissions: r
      state: present
    when: inventory_hostname == "stapp01"


  - name: Create file story.txt on app server 2
    file:
      path: /opt/data/story.txt
      state: touch
      owner: root
      group: root
    when: inventory_hostname == "stapp02"

  - name: Set ACL for user steve on story.txt
    acl:
      path: /opt/data/story.txt
      entity: steve
      etype: user
      permissions: rw
      state: present
    when: inventory_hostname == "stapp02"


  - name: Create file media.txt on app server 3
    file:
      path: /opt/data/media.txt
      state: touch
      owner: root
      group: root
    when: inventory_hostname == "stapp03"

  - name: Set ACL for group banner on media.txt
    acl:
      path: /opt/data/media.txt
      entity: banner
      etype: group
      permissions: rw
      state: present
    when: inventory_hostname == "stapp03"
```

How to Run

From /home/thor/ansible:

ansible-playbook -i inventory playbook.yml

Verification Commands (If Needed)

On each app server:

# Check file exists
ls -l /opt/data/

# Check ACLs
getfacl /opt/data/blog.txt
getfacl /opt/data/story.txt
getfacl /opt/data/media.txt


---

### Screenshots

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/47e4fe4a-dd8e-4888-8667-f3afb4b7518b" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/7f2b112e-300c-47d8-8d7a-e3be79ecbcb8" />




