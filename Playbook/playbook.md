# Ansible Playbook Examples

## 1. yum.yml

### Playbook

```yaml
---
- name: Install multiple packages
  hosts: webserver
  become: true

  tasks:
    - name: Install packages
      yum:
        name:
          - httpd
        state: present
```

### Description
Installs the Apache HTTP Server (httpd) package on all hosts in the webserver group.

### Run Command

```bash
ansible-playbook -i inventory.ini yum.yml
```

---

## 2. service.yml

### Playbook

```yaml
---
- name: Ensure httpd running
  hosts: webserver
  become: true

  tasks:
    - name: start httpd
      service:
        name: httpd
        state: restarted
        enabled: true

    - name: stop nginx
      service:
        name: nginx
        state: stopped
```

### Description
Restarts and enables Apache (httpd) service and stops Nginx service.

### Run Command

```bash
ansible-playbook -i inventory.ini service.yml
```

---

## 3. shell.yml

### Playbook

```yaml
---
- name: count running process
  hosts: all
  become: true

  tasks:
    - name: count running process
      shell: ps aux | wc -l
      register: process_count

    - name: show process count
      debug:
        msg: "total process: {{ process_count.stdout }}"
```

### Description
Counts the number of running processes on all target servers.

### Run Command

```bash
ansible-playbook -i inventory.ini shell.yml
```

---

## 4. cat_shell.yml

### Playbook

```yaml
---
- name: cat file
  hosts: all
  become: true

  tasks:
    - name: check file info
      shell: cat /etc/environment
      register: cat_file

    - name: print file
      debug:
        var: cat_file.stdout
```

### Description
Displays the contents of the /etc/environment file.

### Run Command

```bash
ansible-playbook -i inventory.ini cat_shell.yml
```

---

## 5. copy.yml

### Playbook

```yaml
---
- name: copy config file
  hosts: web1
  become: true

  tasks:
    - name: copy file src to dest
      copy:
        src: text.txt
        dest: /home/ansible/
        owner: ansible
        group: ansible
        mode: '0644'
```

### Description
Copies a file from the Ansible control node to the target server.

### Run Command

```bash
ansible-playbook -i inventory.ini copy.yml
```

---

## 6. install_nginx.yml

### Playbook

```yaml
---
- name: Web server
  hosts: webserver
  become: yes

  tasks:
    - name: install nginx
      package:
        name: nginx
        state: present

    - name: start nginx
      service:
        name: nginx
        state: started
        enabled: yes
```

### Description
Installs and starts the Nginx web server.

### Run Command

```bash
ansible-playbook -i inventory.ini install_nginx.yml
```

---

## 7. install_nginx-with-copy-module.yml

### Playbook

```yaml
---
- name: install and start nginx on web server
  hosts: webserver
  become: true

  tasks:
    - name: install nginx
      package:
        name: nginx
        state: present

    - name: start and enabled nginx
      service:
        name: nginx
        state: started
        enabled: yes

    - name: Create a custom index page
      copy:
        content: "<h1>Deployed by Ansible</h1>"
        dest: /usr/share/nginx/html/index.html
```

### Description
Installs Nginx, starts the service, and deploys a custom web page.

### Run Command

```bash
ansible-playbook -i inventory.ini install_nginx-with-copy-module.yml
```

---

## 8. directory.yml

### Playbook

```yaml
---
- name: create application directory
  hosts: all
  become: true

  tasks:
    - name: create directory
      file:
        path: /opt/myapp
        state: directory
        owner: ansible
        mode: '0755'
```

### Description
Creates a directory with specified ownership and permissions.

### Run Command

```bash
ansible-playbook -i inventory.ini directory.yml
```

---

## 9. lineinfile.yml

### Playbook

```yaml
---
- name: set timezone in environment
  hosts: web1
  become: true

  tasks:
    - name: set timezone in web1
      lineinfile:
        path: /etc/environment
        line: 'TZ=Asia/Kolkata'
        create: true
```

### Description
Adds the timezone configuration to the environment file.

### Run Command

```bash
ansible-playbook -i inventory.ini lineinfile.yml
```

---
