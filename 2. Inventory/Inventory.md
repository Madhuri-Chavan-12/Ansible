
# INVENTORY 
- An Ansible inventory file contains the list of machines that Ansible manages. These machines are known as *managed nodes*. The inventory file tells Ansible where to run tasks and how to connect to those systems. It also allows grouping of hosts and defining connection variables.
## TYPES OF INVENTORY:
**1. STATIC INVENTORY** : Static inventory is a manually created file where hostnames or IP   addresses are written directly. It is mostly used for small setups, practice environments, or systems where servers rarely change. Static inventory files are commonly written in **INI** or **YAML** format.

1.	Hosts are grouped using square brackets [group_name].
2.	IP addresses or hostnames are listed under groups.
3.	Variables can be defined globally using all:vars.

### STATIC INVENTORY – INI FORMAT:
```
ini
[webservers]
192.168.1.10
192.168.1.11

[dbservers]
192.168.1.20

[all:vars]
ansible_user=ubuntu
ansible_ssh_private_key_file=~/.ssh/id_rsa

```
    
### STATIC INVENTORY – YAML FORMAT:
```
all:
  children:
    webservers:
      hosts:
        web1:
          ansible_host: 192.168.1.10
        web2:
          ansible_host: 192.168.1.11
    dbservers:
      hosts:
        db1:
          ansible_host: 192.168.1.20
  vars:
    ansible_user: ubuntu
    ansible_ssh_private_key_file: ~/.ssh/id_rsa

```

**2. DYNAMIC INVENTORY** : Dynamic inventory is used when servers are created and destroyed frequently, especially in cloud platforms like AWS. Ansible automatically fetches host information using inventory plugins or scripts instead of maintaining a manual list.
WHY INVENTORY IS IMPORTANT? 
- Defines which machines Ansible controls
- Organizes hosts into logical groups
- Stores connection and authentication details
- Enables scalable automation

 #### BEST PRACTICES
- Use groups instead of single hosts
- Store secrets securely using Ansible Vault
- Use YAML for complex inventories
- Prefer dynamic inventory for cloud environments

```
ansible -i inventory_file_name -m ping <adhoc_command>
```
 ex.  ansible -i inventory.ini -m ping all

```
ansible -i inventory.ini -m ping  server_name<ip_addr>
```


