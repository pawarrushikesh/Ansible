# Ansible Lab

This project demonstrates how to set up an AWS EC2 instance using Amazon Linux 2 and manage it with Ansible using password-based SSH authentication.

## Architecture

```text
+-------------------+
|   Control Node    |
| (Ansible Installed)|
+---------+---------+
          |
          | SSH
          |
+---------v---------+
| AWS EC2 Instance  |
| Amazon Linux 2    |
| User: ansible     |
+-------------------+
```

## Prerequisites

### AWS

- AWS Account
- EC2 Instance (Amazon Linux 2)
- Security Group with Port 22 open

### Control Node

- Linux / WSL / Ubuntu
- Python 3
- Ansible
- sshpass
- Git

## EC2 User Data Script

The following script is used during EC2 launch to create an Ansible user and enable password authentication.

```bash
#!/bin/bash

# Create ansible user
useradd -m -s /bin/bash ansible

# Set password
echo "ansible:ansible123" | chpasswd

# Add to wheel group
usermod -aG wheel ansible

# Passwordless sudo
echo "ansible ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/ansible
chmod 440 /etc/sudoers.d/ansible

# Enable password authentication
sed -i 's/^PasswordAuthentication.*/PasswordAuthentication yes/' /etc/ssh/sshd_config

# Disable challenge response
sed -i 's/^ChallengeResponseAuthentication.*/ChallengeResponseAuthentication no/' /etc/ssh/sshd_config

# Ensure PAM is enabled
sed -i 's/^UsePAM.*/UsePAM yes/' /etc/ssh/sshd_config

# Restart SSH
systemctl restart sshd
```

## Repository Structure

```text
ansible-aws-lab/
│
├── inventory
├── ansible.cfg
├── README.md
│
└── playbooks/
    └── ping.yml
```

## Install Ansible

### Ubuntu / WSL

```bash
sudo apt update
sudo apt install python3-pip sshpass -y
pip3 install ansible
```

### Verify Installation

```bash
ansible --version
```

## Inventory File

Create an `inventory` file.

```ini
[linux]
PUBLIC_IP_ADDRESS

[linux:vars]
ansible_user=ansible
ansible_password=ansible
ansible_connection=ssh
ansible_become=true
ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```

Replace:

```text
PUBLIC_IP_ADDRESS
```

with your EC2 public IP.

## Ansible Configuration

Create `ansible.cfg`.

```ini
[defaults]
inventory = inventory
host_key_checking = False
remote_user = ansible
```

## First Playbook

Create `playbooks/ping.yml`.

```yaml
---
- name: Test connectivity
  hosts: linux
  gather_facts: false

  tasks:
    - name: Ping remote host
      ping:
```

## Validate Connectivity

Run ad-hoc command:

```bash
ansible linux -m ping
```

Expected output:

```json
SUCCESS => {
    "ping": "pong"
}
```

Run playbook:

```bash
ansible-playbook playbooks/ping.yml
```

## Git Commands

Initialize repository:

```bash
git init
git add .
git commit -m "Initial commit"
```

Connect GitHub repository:

```bash
git remote add origin https://github.com/<username>/ansible-aws-lab.git
git branch -M main
git push -u origin main
```

## Future Enhancements

- Ansible Roles
- Dynamic AWS Inventory
- Ansible Vault
- Nginx Deployment
- Apache Deploym*nt
-*User*Management Playbooks
- GitHub Acti*ns CI/CD
- Terraform Integration
-*Multi-EC2 Management

*# Learning Objectives

*fter completing this lab, you will*be able to:

- Create AWS EC2 inst*nces
-*Configure SSH access
- Install and*configure Ansible
-*Manage*Linux servers using Ansible
-*Execute*Playbooks
- Use Inventory files
- *tore Infrastructure as Code in Git*ub

## Author

**Rushikesh Pawar***
Senior Consultant | Azure & DevOp* Enthusiast

---
⭐ If this project*helped you learn Ansible, consider*giving it a star.
