# Variable Precedence

Variable precedence defines which variable value Ansible uses when the same variable name is defined in multiple places.

👉 Higher precedence overrides lower precedence.


### Why Variable Precedence Matters ? 

* Prevents unexpected overrides

* Helps design clean roles

* Critical for debugging

#### Variable Precedence Order (Low → High)
**1. Role Defaults (Lowest Priority)**
```
roles/myrole/defaults/main.yml
```

 - Used for safe default values

- Always overridden by anything else


**2. Inventory Variables**  

   a) Group Variables
```
inventory/group_vars/web.yml
```
b) Host Variables
```
inventory/host_vars/web1.yml
```

- Host vars override group vars

**3. Playbook Variables**

Defined inside playbook:
```
vars:
  app_port: 9090
```

**4. Vars Files**

Included via vars_files
```
vars_files:
  - vars.yml
```

**5. Role Vars**
```
roles/myrole/vars/main.yml
```


**6. Task Variables**

Defined at task level:
```
- name: Start service
  service:
    name: nginx
    state: started
  vars:
    app_port: 7070
```
**7. Block Variables**
```
- block:
    - debug:
        msg: "{{ app_port }}"
  vars:
    app_port: 6060
```

**8. Registered Variables**
```
- command: uptime
  register: uptime_output
```
**9. Set Facts**
```
- set_fact:
    app_port: 5050
```

**10. Extra Vars (Highest Priority)**

Passed via CLI:
```
ansible-playbook site.yml -e "app_port=4040"
```

- 🔥 Nothing overrides Extra Vars
---
### Simplified Precedence Summary
  
| Priority |   	Variable Source  |  
|----------|------------------  |  
| Lowest   |  	  Role defaults |  
| ↑	&emsp;&emsp; |Inventory vars  |   
| ↑	&emsp;&emsp;|Play vars  |   
| ↑	&emsp;&emsp;|Role vars  |  
| ↑	&emsp;&emsp;|Task / Block vars  |   
| ↑	&emsp;&emsp;|set_fact  |   
Highest &emsp;	|Extra vars  |   



