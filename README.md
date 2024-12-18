# Ansible
- Ansible is a configuration management tool that uses a push-based approach, written in Python. It utilizes YAML files for configuration and works over SSH.

- Ansible uses inventory files to define the hosts and groups of hosts that you want to manage. These files can be static (a plain text file listing IP addresses or hostnames) or dynamic (generated from cloud providers or other systems).

- A simple inventory file 
    ```bash
    [webservers]
    web1 ansible_host=192.168.1.3 ansible_user=ubuntu ansible_ssh_private_key_file=/path/to/key
    web2 ansible_host=192.168.1.4 ansible_user=ubuntu ansible_ssh_private_key_file=/path/to/key

    [dbservers]
    db1 ansible_host=192.168.1.5 
    db2 ansible_host=192.168.1.6 

    [dbservers:vars]
    ansible_user=root
    ansible_ssh_private_key_file=/path/to/key
    ```



### Three Ways to Configure a System with Ansible
#### 1. Ad-hoc Commands
Ad-hoc commands are single-line commands used for quick, one-off tasks on managed nodes. These are simple to use but lack idempotency (i.e., they do not ensure the desired state is maintained if re-run).

```bash
ansible group1 -a "ls -a"  # Executes 'ls -a' on all nodes in group1
ansible group1 -ba "apt-get update"  # Runs with elevated privileges (sudo)
```

#### 2. Modules
Modules are Ansible programs designed to perform specific tasks. Each module serves a distinct purpose, such as managing packages, files, or services.

```bash
ansible all -m command -a "ls -la"  # Uses the 'command' module to list directory contents
```
#### 3. Playbooks
Playbooks are YAML files that contain a collection of tasks, often using multiple modules, to define the desired state of a system. They enable complex automation workflows and ensure idempotency by verifying the system's state.



### Ansible Commands
```bash

ansible <group> -m setup   # Gathers and displays system information (facts) from all hosts in the specified group.

ansible <group> --list-hosts # Lists all hosts that belong to the specified group in the inventory file.

ansible all -m ping    # Uses the ping module to test connectivity to all hosts in the inventory.

ansible-playbook --syntax-check playbook.yml  #Checks the syntax of a playbook without executing it.

ansible-playbook playbook.yml -v  # Runs a playbook with verbose output.
```

