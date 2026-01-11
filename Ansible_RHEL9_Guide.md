# Ansible Installation and Configuration Guide (RHEL 9)

This guide details the installation and configuration of **Ansible on Red Hat Enterprise Linux (RHEL) 9**, including SSH preparation, playbook creation, and automated scheduling.

---

## I. SSH Preparation (Target Worker Nodes)

Before installing Ansible, all worker nodes must be configured to allow **root access** and **password authentication**.

### Switch to Root User
```bash
sudo -i
```

### Set Root Password
```bash
passwd root
```

### Edit SSH Configuration
```bash
vi /etc/ssh/sshd_config
```

Uncomment or set:
```
PermitRootLogin yes
PasswordAuthentication yes
```

### Restart SSH Service
```bash
systemctl restart sshd
```

---

## II. SSH Execution (Master Node Only)

### Generate SSH Keys
```bash
ssh-keygen
```

### Copy Key to Worker Nodes
```bash
ssh-copy-id root@<Worker_Node_Private_IP>
```

Example:
```bash
ssh-copy-id root@172.31.18.172
```

### Verify Connection
```bash
ssh root@172.31.18.172
```
Exit back to master:
```bash
exit
```

---

## III. Ansible Installation (Master Node)

```bash
ansible --version
dnf install -y ansible-core
ansible --version
```

---

## IV. Ansible Configuration

Edit inventory file:
```bash
vi /etc/ansible/hosts
```

Add:
```ini
[webgroup]
172.31.18.172
```

Test:
```bash
ansible all --list-hosts
ansible all -m ping
```

---

## V. Ansible Playbooks

### 1. Install Tree & Tomcat
Create `install.yaml`:

```yaml
- hosts: all
  become: true
  tasks:
    - name: Install Tree and Tomcat
      yum:
        name: "{{ item }}"
        state: present
      with_items:
        - tree
        - tomcat
```

Run:
```bash
ansible-playbook install.yaml
```

---

### 2. Multi‑OS Playbook (Ubuntu & RHEL)

```yaml
- name: Multi-Platform Setup
  hosts: all
  become: true
  tasks:
    - name: Install PHP on Ubuntu
      apt:
        name: php
        state: present
      when: ansible_distribution == 'Ubuntu'

    - name: Install MySQL on RedHat
      yum:
        name: mysql
        state: present
      when: ansible_distribution == 'RedHat'
```

---

## VI. Automation with Cron

Edit crontab:
```bash
crontab -e
```

Run playbook every minute:
```bash
* * * * * /usr/bin/ansible-playbook /etc/ansible/multiplepck.yaml
```

---

End of Documentation
