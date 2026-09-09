# 🧩 Ansible Modules

Ansible modules are the building blocks used to perform tasks on managed hosts. Below are some commonly used modules with practical examples.

---

## 📌 Shell Module

Execute shell commands on remote servers.

```bash
ansible all -i inventory.ini -m shell -a "cat /etc/hostname"
```


---

## 📌 Command Module

Execute commands on remote servers.

```bash
ansible all -i inventory.ini -m command -a "df -h"
```

The `command` module is more secure than `shell` because it does not process shell operators such as `|`, `>`, or `&&`.

---

## 📌 Service Module

Check or manage services on remote hosts.

```bash
ansible all -i inventory.ini -m package -a "name=httpd state=present"  -u ansible -b
```

Start a service:

```bash
ansible all -i inventory.ini -m service -a "name=httpd state=started"
```

Enable service at boot:

```bash
ansible all -i inventory.ini -m service -a "name=httpd enabled=yes"
```

---

## 📌 Shell Module Example

Check server uptime.

```bash
ansible all -i inventory.ini -m shell -a "uptime"
```

---

## 📌 File Module

Create a file on all managed hosts.

```bash
ansible all -i inventory.ini -m file -a "path=/tmp/testfile state=touch mode=0755"
```

Verify the file:

```bash
ansible all -i inventory.ini -m shell -a "ls -l /tmp/testfile"
```

Copy the file:

```bash
ansible all -i inventory.ini -m copy -a "src=/tmp/app.sh dest=/tmp/app" -u ansible
```
User create:

```bash
ansible all -i inventory.ini -m user -a "name=devops state=present"  -u ansible -b
```
print message

```bash
ansible all -i inventory.ini -m debug -a "msg='hello rk'"   -u ansible 
```

---

## ✅ Modules Covered

| Module | Purpose |
|----------|----------|
| shell | Execute shell commands |
| command | Execute commands securely |
| service | Manage Linux services |
| file | Create, modify, or delete files/directories |

These modules form the foundation of day-to-day Ansible administration and automation tasks.

---
