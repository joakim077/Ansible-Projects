# Ansible

### 1.Ansible Variables

```bash
- hosts: all
  vars:
    my_var: "Hello from Ansible!"

  tasks:
    - name: Print a variable
      debug:
        msg: "{{ my_var }}"
```

### 2.Sudo with Ansible
- The --ask-become-pass option in Ansible prompts for the password required for privilege escalation during playbook execution.
- `ansible-playbook --ask-become-pass playbook.yml`

```bash
---
- hosts: all
  become: true 
  tasks:
    - name: Run command as root
      command: touch /tmp/test_file
  
```

### 3.Ansible Tags
Ansible, tags are markers that you can add to individual tasks within a playbook. These tags allow you to selectively execute or skip specific tasks during playbook runs.

```bash
---
- hosts: all
  tasks:
    - name: Install package
      apt: 
        name: httpd
        state: present
      tags: install   
```
- `ansible-playbook my_playbook.yml --list-tags` : List all tags 

- `ansible-playbook my_playbook.yml --tags tag1,tag2,tag3` : run all tasks tagged with "tag1", "tag2", or "tag3".

- `ansible-playbook my_playbook.yml --skip-tags not_for_production` : Skip

### 4.Ansible Handler
Handlers: Handlers are special tasks that are executed only when triggered by another task using the notify keyword.
```bash
- name: Firewall changes
  hosts: all
  become: true

  tasks:
  - name: Enable a service in firewalldd
    firewalld:
      port: 80/tcp
      permanent: true
      state: disabled
    notify: 
      - Reload firewalld

  handlers:
  - name: Reload firewalld
    service:
      name: firewalld
      state: reloaded  
```

### 5.Ansible Condition


```bash
---
- hosts: all
  tasks:
    - name: Start httpd service
      service:
        name: httpd
        state: started
      when: ansible_os_family == 'Debian' or ansible_os_family == 'RedHat' 

# ansible -m setup <hostname> or ansible all -m setup
```

### 6.Loop in Ansible
Ansible loops allow you to execute a task multiple times, iterating over a collection of items
```bash
---
---
- hosts: all
  tasks:
    - name: Create users
      user: 
        name: "{{ item.name }}"      # item: special variable represents the current item in the loop
        comment: "{{ item.comment }}" 
      loop:                           # loop: keyword introduces the loop.   
        - { name: 'user1', comment: 'First User' }
        - { name: 'user2', comment: 'Second User' }


---
- hosts: all
  vars:
    - apps: [nginx,tomcat,vim]
  tasks:
    - name: Install Programs
      yum:
        name: "{{ item }}"
        state: present
      tags: install_programs
      with_items: "{{ apps }}"
```


