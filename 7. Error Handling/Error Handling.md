# Error Handling in Ansible

Ansible stops execution when a task fails by default. Error handling mechanisms allow you to control failures, continue execution, or take corrective actions.

### 1. Default Behavior

- If a task fails → playbook stops immediately

- Failure means:

  1. Non-zero exit code

   2. Module reports failed: true
```
- name: Install package
  yum:
    name: httpd
    state: present
```

- If this fails, the play ends.
---
### 2. ignore_errors

- Allows playbook execution to continue even if the task fails.
```
- name: Stop service
  service:
    name: nginx
    state: stopped
  ignore_errors: yes
```
---

### 3. failed_when

Defines custom failure conditions.
```
- name: Run command
  command: cat /tmp/test.txt
  register: output
  failed_when: "'No such file' in output.stderr"
```

- Useful when:

   1. Command returns 0 but output indicates failure

   2. You want stricter validation
---
### 4. changed_when

- Controls when Ansible reports a task as changed.
```
- name: Check file
  command: ls /tmp/test.txt
  register: output
  changed_when: false
```

- Used to:

  1. Avoid false "changed" status

   2. Keep idempotency clean
---

### 5. block / rescue / always

- Used for structured error handling (try-catch style).

Example:
```
- block:
    - name: Install nginx
      yum:
        name: nginx
        state: present

    - name: Start nginx
      service:
        name: nginx
        state: started

  rescue:
    - name: Handle failure
      debug:
        msg: "Nginx installation failed"

  always:
    - name: Cleanup
      debug:
        msg: "This runs always"
```
---

### 6. any_errors_fatal

- Stops play execution on any host failure.
```
- hosts: web
  any_errors_fatal: true
```

- Without this: Ansible continues on other hosts

- With this:
One host fails → all stop
---
### 7. max_fail_percentage

- Stops play when failures cross a threshold.
```
- hosts: web
  max_fail_percentage: 30
```

- If more than 30% hosts fail → play stops.

- Useful in large inventories.
---
### 8. check_mode and error handling

- Some tasks behave differently in --check mode.
```
- name: Create file
  file:
    path: /tmp/test.txt
    state: touch
  check_mode: no
```

- Forces task to run even in check mode.
---
### Quick Comparison  
| Method	&nbsp;&nbsp;&nbsp;&emsp;&emsp;&emsp;&emsp;    &emsp; | Purpose &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;  |      
|--------------------------|-----------------------------|          
| ignore_errors	&nbsp;&nbsp;&nbsp;&emsp;&emsp; &emsp;| Continue despite failure  &nbsp; |      
| failed_when	&nbsp;&nbsp;&emsp;&emsp;&emsp; &emsp;| Custom failure logic &emsp;&nbsp;&nbsp;&nbsp;&nbsp;  |     
| changed_when	&nbsp;&nbsp;&emsp;&emsp;&emsp;| Control change status &emsp; &nbsp;|    |     
| block/rescue	&emsp; &emsp;&emsp;&emsp;| Structured error handling  &nbsp; |        
| any_errors_fatal&emsp;	&emsp;&emsp; |  Stop all hosts on failure  &nbsp;&nbsp;&nbsp; |       
| max_fail_percentage	&emsp; | Control failure tolerance  &nbsp; &nbsp;|      
