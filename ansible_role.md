## Ansible Roles

- Ansible Roles are a way to organize and structure your playbooks and configuration files. They allow you to group tasks, handlers, variables, templates, and files into reusable and modular components.

- `ansible-galaxy init role_name`  : Initialize a role

- Role Directory Structure
    ```bash
    role_name/
    ├── defaults
    │   └── main.yml # Default variables for the role
    ├── handlers
    │   └── main.yml # Handlers, which can be used by this role or even anywhere outside this role
    ├── meta
    │   └── main.yml # Metadata for the role, including author, support details, and dependencies
    ├── README.md # Optional: A human-readable description of the role and its requirements
    ├── tasks
    │   └── main.yml # The main list of tasks that the role executes
    ├── files/
    │  └── nginx.conf # Static files that need to be copied to the remote hosts.
    ├── templates
    │   └── ... # Template files, which use Jinja2 templating language
    ├── tests
    │   ├── inventory # Inventory file for testing the role
    │   └── test.yml # Test playbook for the role
    └── vars
        └── main.yml # Other variables for the role
    ```

- Use Role
    ```bash
        ---
        - name: Install httpd and Firewall changes
          hosts: all
          roles:
            - httpd_setup
            - fwd_service 
    ```