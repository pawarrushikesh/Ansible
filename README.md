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
