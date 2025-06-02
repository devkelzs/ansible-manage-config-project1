# Basic Server Hardening with Ansible

## 🛡️ Overview

This Ansible project automates **server hardening** for Debian-based systems (like Ubuntu) and CentOS-based systems (including Amazon Linux). It ensures:

- Automatic updates of system packages.
- Disabling root login over SSH.
- Firewall setup with **UFW** (for Debian) and **firewalld** (for RedHat/CentOS).

With this, you achieve consistent, secure, and scalable server configurations across multiple environments.


##  Objectives

-  Enhance server security via package updates, restricted SSH access, and firewall rules.
-  Ensure consistency across environments through automation.
-  Scale configurations across many servers using a single playbook.


## ⚙️ Prerequisites

###  Control Node Requirements

- **Operating System**: Linux (Ubuntu/CentOS) or macOS.
- **Ansible**: Version 2.9 or later.

#### Installation on Ubuntu:
```sudo apt update```
```sudo apt install ansible -y```

#### Verify Installation:
 
```ansible --version```

SSH Access: Your control node must be able to SSH into target servers as a user with sudo privileges (e.g., admin).

#### Generate SSH Key:

```ssh-keygen -t rsa -b 4096```

#### Copy SSH Key to Target Servers:

```ssh-copy-id admin@<server_ip>```

## ✅ Target Server Requirements
Operating Systems: Ubuntu 20.04+, CentOS 7/8, or Amazon Linux.

Open Port: Port 22 must be open and accessible.

SSH: SSH server must be running.

## 🗂 Project Structure

basic-server-hardening/
├── inventory.ini 

├── server_hardening.yml       

└── README.md                  


### Setup Instructions
1. Create the Project Directory

```mkdir -p ~/ansible-projects/basic-server-hardening```
```cd ~/ansible-projects/basic-server-hardening``

2. Install Ansible (if not already installed)
Refer to the Prerequisites section.

3. Set Up SSH Access
Ensure your control node can SSH into each target server using an SSH key.

4. Create the Inventory File
inventory.ini:

[servers]
```192.168.1.10 ansible_os_family=Debian```
```192.168.1.11 ansible_os_family=RedHat```

[servers:vars]
```ansible_user=admin```
```ansible_ssh_private_key_file=~/.ssh/id_rsa```
Replace IPs and OS families with actual values for your servers.
5. Create the Playbook File
Create a new file called server_hardening.yml and write your full Ansible playbook content.

## 📦 Usage
1. Test Connectivity

```ansible -i inventory.ini servers -m ping```
Expected output:

```"ping": "pong"```
2. Run in Check Mode (Dry Run)

```ansible-playbook -i inventory.ini server_hardening.yml --check```
3. Apply the Playbook

```ansible-playbook -i inventory.ini server_hardening.yml```

## 🔍 Verification
### 🖥 On Debian/Ubuntu Hosts:

```sudo apt update && apt list --upgradable```      # No upgrades should be available
```grep PermitRootLogin /etc/ssh/sshd_config```     # Should show 'PermitRootLogin no'
```sudo ufw status ```                         # Should show port 22 allowed and UFW enabled
🖥 On CentOS/Amazon Hosts:
```sudo dnf check-update  ```                       # No updates should be listed
```grep PermitRootLogin /etc/ssh/sshd_config ```    # Should show 'PermitRootLogin no'
```sudo firewall-cmd --list-all  ```                # Should show SSH service enabled


## 🧯 Troubleshooting
### 🔐 SSH Connection Fails?
Ensure:

Port 22 is open.

SSH key is correctly copied.

User has sudo privileges.

ansible_user and ansible_ssh_private_key_file are correct in inventory.ini.

## 🧱 UFW or firewalld issues?
Ensure the relevant package (ufw/firewalld) is installed.

Check Ansible output for skipped/failed tasks.

Use --check or -vvv flags for verbose/debugging output.
