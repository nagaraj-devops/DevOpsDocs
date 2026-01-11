# Ansible Playbook Experience & Assignments

## Best Practices
- Use `--syntax-check` before running playbooks
- Use **Gathering Facts** to collect OS, memory, disk info
- Tasks execute **in sequence**
- Always use `name` for plays and tasks for clarity

---

## Question 1 – Convert Adhoc Command to Playbook

Adhoc:
```bash
ansible <GROUP> -m user -a "name=odoo state=present"
```

Playbook:
```yaml
- name: play for Question 1
  hosts: app1.company.com
  tasks:
    - name: ensure user odoo exists
      user:
        name: odoo
        state: present
```

Run:
```bash
ansible-playbook user.yml
```

Verify on worker:
```bash
id odoo
cat /etc/passwd
```

---

## Question 2 – Directory & File Creation

Adhoc:
```bash
ansible all -m file -a "path=/opt/data state=directory"
ansible all -m copy -a "content='TopSecret' dest='/opt/data/secret.txt'"
```

Playbook:
```yaml
- name: play for Question 2
  hosts: all
  tasks:
    - name: directory exists
      file:
        path: /opt/data
        state: directory

    - name: secret file created
      copy:
        content: TopSecret
        dest: /opt/data/secret.txt
```

---

## Deploy Static Website with Nginx

Fork:
```
https://github.com/nramnad/ansible-example-static-website
```

Playbook:
```yaml
- hosts: all
  become: yes
  tasks:
    - name: nginx installed
      package:
        name: nginx
        state: present

    - name: nginx running
      service:
        name: nginx
        state: started

    - name: web root exists
      file:
        path: /var/www/html
        state: directory

    - name: deploy static website
      get_url:
        url: https://raw.githubusercontent.com/nramnad/ansible-ci-cd/main/index.html
        dest: /var/www/html/index.html
```

---

End of Playbook Lab
