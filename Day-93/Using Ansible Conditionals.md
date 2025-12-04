# Day 93: Using Ansible Conditionals - KodeKloud 100 Days DevOps Challenge

This guide documents how to use **Ansible conditionals** to copy files to specific servers with different ownership and permissions.

---

## Task Description

We need to perform the following tasks using **Ansible `when` conditionals**:

1. Copy `blog.txt` from `/usr/src/data` on the jump host to `/opt/data` on **App Server 1**.  
   - Owner/Group: `tony`  
   - Permissions: `0755`

2. Copy `story.txt` from `/usr/src/data` on the jump host to `/opt/data` on **App Server 2**.  
   - Owner/Group: `steve`  
   - Permissions: `0755`

3. Copy `media.txt` from `/usr/src/data` on the jump host to `/opt/data` on **App Server 3**.  
   - Owner/Group: `banner`  
   - Permissions: `0755`

**Note:** Use the `ansible_nodename` variable with `when` conditions, and run the playbook for **all hosts**.

---
## Step 1: Verify Hosts in Inventory

```bash
cd /home/thor/ansible
cat inventory
```

Example inventory might contain:

```
stapp01
stapp02
stapp03
```

---

## Step 2: Check if ansible_nodename is Defined
ansible all -i inventory -m debug -a "var=ansible_nodename"


Output likely shows VARIABLE IS NOT DEFINED!.
This is because ansible_nodename is not automatically defined in modern Ansible versions.

---

## Step 3: Create Playbook

Create the playbook at /home/thor/ansible/playbook.yml:
```
---
- name: Copy files to App Servers using conditionals
  hosts: all
  become: yes
  gather_facts: yes
  tasks:

    - name: Define ansible_nodename fact
      set_fact:
        ansible_nodename: "{{ ansible_hostname }}"

    - name: Copy blog.txt to App Server 1
      copy:
        src: /usr/src/data/blog.txt
        dest: /opt/data/blog.txt
        owner: tony
        group: tony
        mode: '0755'
      when: ansible_nodename == "stapp01"

    - name: Copy story.txt to App Server 2
      copy:
        src: /usr/src/data/story.txt
        dest: /opt/data/story.txt
        owner: steve
        group: steve
        mode: '0755'
      when: ansible_nodename == "stapp02"

    - name: Copy media.txt to App Server 3
      copy:
        src: /usr/src/data/media.txt
        dest: /opt/data/media.txt
        owner: banner
        group: banner
        mode: '0755'
      when: ansible_nodename == "stapp03"
```

Step 4: Verify ansible_nodename After set_fact
ansible all -i inventory -m debug -a "var=ansible_nodename"


Expected output:

stapp01 | SUCCESS => { "ansible_nodename": "stapp01" }
stapp02 | SUCCESS => { "ansible_nodename": "stapp02" }
stapp03 | SUCCESS => { "ansible_nodename": "stapp03" }

## Step 5: Run the Playbook
```
ansible-playbook -i inventory playbook.yml
```

Expected output:
```
TASK [Copy blog.txt to App Server 1]   changed: [stapp01]
TASK [Copy story.txt to App Server 2]  changed: [stapp02]
TASK [Copy media.txt to App Server 3]  changed: [stapp03]
```

changed: [host] indicates the file was successfully copied.

Skipped tasks on other hosts are expected due to when conditions.

---

## Step 6: Verify Files on App Servers
```
ssh stapp01
ls -l /opt/data/blog.txt

# Check owner/group and permissions (should be tony:tony 0755)

ssh stapp02
ls -l /opt/data/story.txt

# Check owner/group and permissions (should be steve:steve 0755)

ssh stapp03
ls -l /opt/data/media.txt

# Check owner/group and permissions (should be banner:banner 0755)

```

---

### Screenshots

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/874e67e3-f284-45e1-9f1f-701aedd43c9e" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/f073ad33-af57-4bcc-a674-661e635ed5dc" />


---




