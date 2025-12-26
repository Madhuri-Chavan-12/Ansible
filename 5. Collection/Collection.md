# Ansible Collections 



An **Ansible Collection** is a structured packaging format that bundles **roles, modules, plugins, playbooks, and documentation** into a single, versioned, reusable unit.

Collections were introduced to:

* Avoid clutter in the global Ansible namespace
* Enable proper versioning
* Make content easier to share and reuse
* Support Ansible Automation Hub and Ansible Galaxy



## Why Collections?

* Clean architecture
* Reusability across projects
* Enterprise-ready Ansible setups



## Collection Structure

```
my_namespace/my_collection/
├── docs/
├── galaxy.yml
├── plugins/
│   ├── modules/
│   ├── lookup/
│   └── filter/
├── roles/
│   └── my_role/
├── playbooks/
├── tests/
└── README.md
```

### Key Files

* **galaxy.yml** → Metadata (name, version, author, dependencies)
* **README.md** → Usage documentation
* **plugins/** → Custom modules & plugins
* **roles/** → Reusable roles



## Naming Convention

```
<namespace>.<collection>
```

Example:

```
community.general
mycompany.devops
```



## Creating a Collection

```bash
ansible-galaxy collection init my_namespace.my_collection
```

This command generates the full directory structure automatically.



## Using a Collection in a Playbook


```yaml
- hosts: all
  collections:
    - my_namespace.my_collection
  roles:
    - my_role
```



## Installing a Collection

From Ansible Galaxy:

```bash
ansible-galaxy collection install community.general
```

From a Git Repository:

```bash
ansible-galaxy collection install git+https://github.com/org/repo.git
```




## Publishing a Collection

1. Build the collection

   ```bash
   ansible-galaxy collection build
   ```

2. Publish

   ```bash
   ansible-galaxy collection publish my_collection-1.0.0.tar.gz
   ```

Requires Ansible Galaxy token.



## Collections vs Roles (Quick Comparison)

| Aspect           | Role    | Collection                        |  
| ---------------- | ------- | --------------------------------- |  
| Scope            | Limited | Broad (roles + modules + plugins) |  
| Versioning       | Weak    | Strong                            |  
| Reusability      | Medium  | High                              |  
| Enterprise Ready | ❌       | ✅                                 |


