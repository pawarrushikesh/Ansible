# 🚀 Ansible Lab

This project demonstrates how to set up an AWS EC2 instance using Amazon Linux 2 and manage it with Ansible using password-based SSH authentication.

---

## 🏗️ Architecture

```text
+--------------------+
|    Control Node    |
| (Ansible Installed)|
+---------+----------+
          |
          | SSH
          |
+---------v----------+
|  AWS EC2 Instance  |
|   Amazon Linux 2   |
|    User: ansible   |
+--------------------+
```

---

## ✅ Prerequisites

### AWS

- AWS Account
- EC2 Instance
- Linux AMI (Amazon Linux 2)
- Security Group with Port 22 open
- SSH access enabled
- No EC2 Key Pair required

### Control Node

- Linux / WSL / Ubuntu
- Python 3
- Ansible
- sshpass
- Git

---

## ☁️ Launch EC2 Instance

Go to:

```text
AWS Console → EC2 → Launch Instance
```

### Select AMI

```text
Amazon Linux 2
```

### Select Instance Type

```text
t3.micro
```

### Key Pair

In the Key Pair section:

```text
Key Pair Name:
Proceed without a key pair
```

### Configure Security Group

```text
Allow SSH access (Port 22)
```

---

## 📝 Add User Data

Paste the following script into the **User Data** section.

### EC2 User Data Script

The following script is used during EC2 launch to create an Ansible user and enable password authentication.

```bash
# Create ansible user
sudo useradd ansible

#!/bin/bash

# Create ansible user
useradd -m -s /bin/bash ansible

# Set password
echo "ansible:ansible123" | chpasswd

# Grant passwordless sudo
echo 'ansible ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

# Enable password authentication
sed -i 's/^PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config
sed -i 's/^#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config

# Restart SSH service
sudo systemctl restart sshd
```

---

## 🔐 Connect Using SSH

After the instance is running, copy the Public IPv4 address.

From your terminal:

```bash
ssh ansible@<PUBLIC_IP>
```

Example:

```bash
ssh ansible@54.xx.xx.xx
```

---

## ⚙️ Install Ansible

```bash
sudo dnf update -y
sudo dnf install ansible-core -y
ansible --version
ansible-playbook --version
```
---

## ⚙️ Configure Ansible

### Disable Host Key Checking

Create or edit the Ansible configuration file:

```bash
vi ansible.cfg
```

Add the following content:

```ini
[defaults]
host_key_checking = False
```

This prevents SSH host key verification prompts when connecting to new servers.

---

## 📋 Create Inventory File

Create the inventory file and add your instance IP addresses:

```bash
vi inventory.ini
```

```ini
[webserver]
web1 ansible_host=3.85.18.51
web2 ansible_host=34.228.37.22

[dbserver]
db1 ansible_host=18.209.65.23
db2 ansible_host=54.235.52.68

[all:vars]
ansible_python_interpreter=/usr/bin/python3
```

### Why Specify Python Interpreter?

Adding the following line:

```ini
ansible_python_interpreter=/usr/bin/python3
```

helps eliminate warnings similar to:

```text
[WARNING]: Platform linux on host db2 is using the discovered Python interpreter at /usr/bin/python3.x, but future installation of another Python interpreter could change the meaning of that path.
```

---

## 🚀 First Ansible Command

Verify connectivityaged hosts using the Ansible user and password authentication:

```bash
ansible all -i inventory.ini -m ping -u ansible -k
```

You will be prompted for the SSH password:

```text
SSH password:
```

Expected output:

```text
web1 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}

web2 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}

db1 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}

db2 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

✅ If you receive **pong** from all hosts, your Ansible control node is successfully communicating with the managed servers.

---
## 🚀 Future Enhancements

- Ansible Roles
- Dynamic AWS Inventory
- Ansible Vault
- Nginx Deployment
- Apache Deployment
- User Management Playbooks
- GitHub Actions CI/CD
- Terraform Integration
- Multi-EC2 Management

---

## 🎯 Learning Objectives

After complet*ng this lab, you will be able to:
*- Create AWS EC2 instances
- Confi*ure SSH access
- Install and confi*ure Ansible
- Manage Linux servers using Ansible
- Execute Playbooks
- Use Inventory files
- Store Infrastructure as Code (IaC) in GitHub

---

## 📂 Project Structure

```text
ansible-lab/
│
├── README.md
├── inventory
├── ansible.cfg
├── playbooks/
│   └── site.yml
├── roles/
└── group_vars/
```

---

## 👨‍💻 Author

**Rushikesh Pawar**  
Senior Consultant | Azure & DevOps Enthusiast

https://img.shields.io/badge/GitHub-rushikeshpawar-blue?style=for-the-badge&logo=github](https://github.com/)

---

⭐ If this project helped you learn Ansible, consider giving it a **Star** on GitHub.

Happy Automating! 🚀
