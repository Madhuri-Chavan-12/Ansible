# Ansible Playbook 



An Ansible Playbook is a YAML file used to define automation tasks. It describes what configuration or action should be applied, on which hosts, and in what order.

Playbooks are the core of Ansible automation and are used for:

* Server configuration
* Application deployment
* Infrastructure provisioning
* Continuous delivery



## Why Use Playbooks?

* **Readable & simple** (YAML-based)
* **Agentless** (uses SSH)
* **Idempotent** (safe to run multiple times)
* **Version controllable** (ideal for Git)



## Playbook File Structure

```yaml
---
- name: Playbook name
  hosts: target_hosts
  become: yes
  tasks:
    - name: Task name
      module_name:
        option: value
```


## Key Components Explained

### 1. `name`
Describes the playbook or task purpose (for readability).

### 2. `hosts`

Defines the target machines.
  
Examples:

* `all`
* `webservers`
* `db`

### 3. `become`

Used for privilege escalation (sudo).

```yaml
become: yes
```

### 4. `tasks`

A list of actions executed sequentially.


## Commonly Used Modules

### `yum` / `apt`

Install packages.

```yaml
- name: Install nginx
  yum:
    name: nginx
    state: present
```

### `service`

Manage services.

```yaml
- name: Start nginx
  service:
    name: nginx
    state: started
```

### `copy`

Copy files to remote hosts.

```yaml
- name: Copy config file
  copy:
    src: nginx.conf
    dest: /etc/nginx/nginx.conf
```

### `file`

Manage file permissions and directories.

```yaml
- name: Create directory
  file:
    path: /opt/app
    state: directory
```


## Variables in Playbooks

Variables help make playbooks reusable.

```yaml
vars:
  package_name: nginx
```

Usage:

```yaml
name: "Install {{ package_name }}"
```

## Handlers

Handlers run **only when notified** (used for restart actions).

```yaml
handlers:
  - name: restart nginx
    service:
      name: nginx
      state: restarted
```

Notify from task:

```yaml
notify: restart nginx
```

## Inventory File Example

```ini
[web]
192.168.1.10
192.168.1.11
```

## Running a Playbook

```bash
ansible-playbook playbook.yml -i inventory
```

With sudo:

```bash
ansible-playbook playbook.yml -i inventory --become
```


## Example Simple Playbook

```yaml
---
- name: Install and start nginx
  hosts: web
  become: yes

  tasks:
    - name: Install nginx
      yum:
        name: nginx
        state: present

    - name: Start nginx service
      service:
        name: nginx
        state: started
```

