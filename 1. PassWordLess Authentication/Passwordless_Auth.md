# Passwordless Authentication in Ansible

- Passwordless authentication in Ansible means Ansible connects to managed nodes using SSH keys instead of passwords.
Once configured, Ansible can run playbooks without asking for a password every time, which is exactly how automation is supposed to work.

- If your Ansible setup still asks for passwords, you’re doing it wrong.

- Automation-friendly – no human interaction required

- More secure than passwords

- Faster execution of playbooks


**How Passwordless Authentication Works** 

 1. Ansible Control Node generates an SSH key pair

2. The public key is copied to all managed nodes

3. The private key stays securely on the control node

4. SSH trusts the key → Ansible connects automatically

**Step-by-Step Setup**

Step 1: Generate SSH Key on Ansible Control Node
```
ssh-keygen -t rsa -b 4096
```

Step 2: Copy Public Key to Managed Nodes
```
ssh-copy-id user@managed-node-ip
```
Step 3: Test Passwordless Login
```
ssh user@managed-node-ip
```
