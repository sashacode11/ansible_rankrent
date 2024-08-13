# ansible_rankrent

Steps from install and automate ansible

workstation:
- set ssh connection
	sudo apt install openssh-server
		answer yes on each
	connect to another ssh's host: ssh <username of host want to connect>@<that ip address>
		answer yes on each
	check ssh key: ls -la .ssh
	generate ssh key for default use, and it need to set passphrase: ssh-keygen -t ed25519 -C "sasha default"
	check ssh key: ls -la .ssh	
	check public key: cat .ssh/id_ed25519.pub
	check private key: cat .ssh/id_ed25519
	Copy the ssh key to the server(s): ssh-copy-id -i ~/.ssh/id_ed25519.pub <host username>@<IP Adderss>
	
- set ssh key use for ansible and don't set passphare: 
	ssh-keygen -t ed25519 -C "ansible"
	ssh-copy-id -i ~/.ssh/ansible.pub <username>@<ipaddress>
		no passpharse
	connect/remotely drop to that host/server: ssh -i .ssh/ansible <username>@<IP Address>
	
- To not enter passphare when connect to host: 
	eval $(ssh-agent)
	ssh-add

host:
- check authorized key and it should same with public key in workstation: 
	cat .ssh/authorized_keys
