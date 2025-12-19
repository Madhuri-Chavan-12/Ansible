# Ansible Roles

Ansible Roles provide a standardized directory structure to organize automation code.
They help in managing complex playbooks by separating tasks, variables, handlers, templates, and files into reusable components.

### Why Use Ansible Roles

- Improves readability of Ansible code

- Encourages reuse across multiple playbooks

- Makes maintenance easier

- Suitable for large and production-grade projects

**Standard Role Directory Structure**
```
roles/
└── webserver/
    ├── tasks/
    │   └── main.yml
    ├── handlers/
    │   └── main.yml
    ├── defaults/
    │   └── main.yml
    ├── vars/
    │   └── main.yml
    ├── templates/
    ├── files/
    ├── meta/
    │   └── main.yml
```

Each directory has a specific purpose, described below.

#### 1. tasks/main.yml

- Contains the list of tasks executed by the role.

```
- name: Install nginx
  apt:
    name: nginx
    state: present

- name: Start nginx service
  service:
    name: nginx
    state: started
```

#### 2. handlers/main.yml

- Handlers are tasks that run only when notified by other tasks.
Commonly used for restarting or reloading services.

```
- name: restart nginx
  service:
    name: nginx
    state: restarted
```

Usage in task:
```
notify: restart nginx
```

#### 3. defaults/main.yml

- Contains default variables for the role.
These variables have the lowest precedence and can be easily overridden.

```
nginx_port: 80
```

#### 4. vars/main.yml

- Contains role variables with higher precedence than defaults.
These variables are less flexible and should be used carefully.

```
nginx_user: www-data
```

#### 5. templates/

- Stores Jinja2 template files used to generate dynamic configuration files.

Example: nginx.conf.j2
```
server {
    listen {{ nginx_port }};
}
```

Used in task:

```
template:
  src: nginx.conf.j2
  dest: /etc/nginx/nginx.conf
```

#### 6. files/

- Contains static files that need to be copied to managed nodes.

Examples:

shell scripts

application binaries

archive files

Usage:
```
copy:
  src: app.war
  dest: /opt/app.war
```

#### 7. meta/main.yml

- Contains metadata about the role, including dependencies.
```
dependencies:
  - role: common
```

### Creating a Role

Roles can be created using the Ansible Galaxy command:

```
ansible-galaxy init webserver
```

This generates the recommended directory structure automatically.

Using a Role in a Playbook:
```
- hosts: web
  become: yes
  roles:
    - webserver
```
