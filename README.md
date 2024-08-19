# ansible_rankrent

Command to run Ansible:
- Run all:
  ```
  ansible-playbook site.yml
  ```
- Check what tags I have:
  ```
  ansible-playbook site.yml --list-tags
  ```
- Run specific tag:
  ```
  ansible-playbook site.yml --tags '<specific tag>'
  ```
- Run specific server:
  ```
  ansible-playbook  site.yml -l <workstations>
  ```
- Run specific server and specific tag:
  ```
  ansible-playbook -i inventory site.yml --limit inquirita.com --tags jenkins
  ```
-------------------

Change or add host/server:
- Inventory:
   - [workstations] local host machine, only set it to add ssh simone user and update cache.
   - [build_servers] host/server which you want to remote install packages.
   - you can add more name as your need, examples: [web_servers],[db_servers]. Remember edit in file site.yml to tell which you want to install in those new added.
------------------
Install Ansible:
```
sudo apt update
sudo apt install ansible
```
------------------

## Part 1: Initial SSH Setup on Your Workstation
Step 1: Install SSH Server
Install the OpenSSH server on your workstation to accept incoming SSH connections:
```
dpkg -s openssh-server | grep Status
#Check if openssh-server installed, if not do install
sudo apt install openssh-server
#Type yes when prompted to confirm the installation.
```

Step 2: Connect to Your Host
Establish an initial SSH connection to your host to trust its SSH key:
```
ssh [username]@[ip address]
#Accept the host's fingerprint by typing yes when prompted.
```

Step 3: Generate and Manage SSH Keys
Check existing SSH keys:
```
ls -la ~/.ssh
``` 
Generate a new SSH key (for general use) with a passphrase:
```
ssh-keygen -t ed25519 -C "yourname default"
``` 
Display your public and private keys:
```
cat ~/.ssh/id_ed25519.pub
```
Copy the public key to your host:
```
ssh-copy-id -i ~/.ssh/id_ed25519.pub [username]@[IP Address]
``` 
Step 4: Create a Dedicated SSH Key for Ansible
Generate a key specifically for Ansible without a passphrase:
```
ssh-keygen -t ed25519 -C "ansible"
``` 
Copy this key to your host:
```
ssh-copy-id -i ~/.ssh/ansible.pub [username]@[IP Address]
```
 
Step 5: Start SSH Agent to Manage Keys
Activate the SSH agent to handle your keys:
```
eval $(ssh-agent)
``` 
Add your key to the agent to avoid passphrase prompts:
```
ssh-add
```
 ---------------------------------------
## Part 2: Adding New Hosts After Setup
Step 1: Update the Inventory File
Ensure your inventory file correctly lists the new host

Step 2: Verify SSH Access
Before running your playbook, manually test SSH access from your Ansible control node to the new host:
```
ssh <new host>
```

Step 3: Copy the ansible public key to the new host:
```
ssh-copy-id -i ~/.ssh/ansible.pub <new host>
```

Step 4:
Run Ansible ad-hoc command to connect with new host
```
ansible-playbook site.yml --limit <new host>

