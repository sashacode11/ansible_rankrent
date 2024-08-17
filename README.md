# ansible_rankrent

Command to run Ansible:
- Run all: ansible-playbook site.yml
- Check what tags I have: ansible-playbook site.yml --list-tags
- Run specific tag: ansible-playbook site.yml --tags 'specific tag'
- Run specific server: ansible-playbook  site.yml -l workstations
-------------------

Change or add host/server:
- Inventory:
   - [workstations] local host machine, only set it to add ssh simone user and update cache.
   - [build_servers] host/server which you want to remote install packages.
   - you can add more name as your need, examples: [web_servers],[db_servers]. Remember edit in file site.yml to tell which you want to install in those new added.
------------------
Install Ansible:
sudo apt update
sudo apt install ansible
------------------

## Part 1: Initial SSH Setup on Your Workstation
Step 1: Install SSH Server
Install the OpenSSH server on your workstation to accept incoming SSH connections:
```
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
Step 1: Prepare the New Host
Install the OpenSSH server:
```
sudo apt install openssh-server
``` 
Ensure the SSH server is running:
```
systemctl status ssh
``` 
Step 2: Copy the Ansible Key to the New Host
Copy the dedicated Ansible SSH key:
```
ssh-copy-id -i ~/.ssh/ansible.pub [username]@[new IP address]
```
Step 3: Verify the Connection
Connect to the new host using the Ansible key to ensure everything is set up correctly:
```
ssh -i ~/.ssh/ansible [username]@[new IP address]
```
