# Install Ansible on Ubuntu 22.04 (Master–Worker Setup)

## Master Node (Ubuntu 22.04)

### EC2 User Data
```bash
#!/bin/bash
sudo su
apt-add-repository ppa:ansible/ansible -y
apt update -y
apt install ansible -y
```

Verify:
```bash
ansible --version
```

---

## Worker Node (Debian 11)

AMI ID:
```
ami-0825877a8b9644990
```

### Configure Root & SSH

```bash
sudo -i
passwd root
cd /etc/ssh
vi sshd_config
```

Uncomment:
```
PermitRootLogin yes
PasswordAuthentication yes
```

Restart:
```bash
systemctl restart sshd
```

---

## Passwordless SSH (Master Only)

Generate key:
```bash
ssh-keygen
```

Copy to worker:
```bash
ssh-copy-id root@<Worker_Private_IP>
```

Verify:
```bash
ssh root@<Worker_Private_IP>
```

Exit:
```bash
exit
```

---

End of Document
