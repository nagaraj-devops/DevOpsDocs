
# Jenkins Distributed Build Setup (Master–Agent Architecture)

This guide explains how to configure **Jenkins Distributed Builds** using AWS EC2 Ubuntu 22.04 instances.

---

## Infrastructure Setup

Create **two AWS EC2 instances** with **Ubuntu 22.04**:

- **Master** – Jenkins Controller
- **Node1** – Jenkins Worker (Agent)

---

## Step 1: Configure Master Node (User Data)

Add the following **User Data** script while launching the **Master** EC2 instance:

```bash
#!/bin/bash
sudo su
apt-get update
apt install openjdk-17-jdk -y
```

---

## Step 2: Verify Java Installation (Master & Node1)

Run on **both Master and Node1**:

```bash
java -version
```

Ensure Java 17 is installed.

---

## Step 3: Install Jenkins on Master Node

Run the following commands on the **Master** node:

```bash
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
```

```bash
sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
```

```bash
apt-get update -y
```

### Fix GPG Key Error (If Occurs)

If you encounter a **GPG / No Public Key** error, copy the public key and apply:

```bash
apt-key adv --keyserver keyserver.ubuntu.com --recv-keys <PUBLIC_KEY>
```

Example:

```bash
apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 5BA31D57EF5975CA
```

Then run:

```bash
apt-get update -y
apt-get install jenkins -y
```

---

## Step 4: Verify Jenkins Service

```bash
systemctl status jenkins
```

Jenkins should be **active (running)**.

---

## Step 5: Access Jenkins UI

Open browser and use:

```
http://<MASTER_PUBLIC_IP_OR_DNS>:8080
```

Example:

```
http://ec2-54-173-81-29.compute-1.amazonaws.com:8080
```

---

## Step 6: Unlock Jenkins

On the **Master node**, obtain the initial admin password:

```bash
cat /var/lib/jenkins/secrets/initialAdminPassword
```

- Copy the password
- Paste it into Jenkins UI
- Click **Continue**
- Install suggested plugins
- Create Jenkins admin user

---

## Step 7: Configure Node1 (Worker Node)

### Set Root Password

On **Node1**:

```bash
passwd root
```

Set and remember the root password.

---

## Step 8: Enable Root SSH Access on Node1

Edit SSH configuration:

```bash
vi /etc/ssh/sshd_config
```

Apply the following changes:

```text
PermitRootLogin yes
PasswordAuthentication yes
#KbdInteractiveAuthentication no
```

Restart SSH service:

```bash
service sshd restart
```

---

## Step 9: Configure Passwordless SSH (Master → Node1)

### Generate SSH Key on Master

```bash
ssh-keygen
```

(Press **Enter** for all prompts)

### Copy SSH Key to Node1

```bash
ssh-copy-id root@<NODE1_PRIVATE_IP>
```

Example:

```bash
ssh-copy-id root@172.31.47.75
```

- Enter Node1 **root password** when prompted
- SSH key gets copied successfully

---

## Step 10: Verify Passwordless SSH

From **Master**:

```bash
ssh root@<NODE1_PRIVATE_IP>
```

✅ You should log in **without password prompt**.

Exit Node1:

```bash
exit
```

---

## Result

- Jenkins Master can communicate with Worker Node
- Passwordless SSH is enabled
- Node1 is ready to be configured as a Jenkins Agent

---

## Next Steps (Optional)

- Add **Node1** in Jenkins:
  - Manage Jenkins → Nodes → New Node
- Configure:
  - Remote root directory
  - Labels
  - Launch agent via SSH
- Run distributed builds

---

✅ End of Jenkins Distributed Build Documentation
