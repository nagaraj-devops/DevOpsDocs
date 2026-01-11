# Chef Installation and Configuration Guide (RHEL 9)

## 1. Environment Setup

Create two AWS EC2 instances with **RedHat Enterprise Linux 9**:

- **Chef Workstation**
- **Node1**

---

## Workstation User Data (Auto Installation)

Apply this script while creating the Chef Workstation EC2:

```bash
#!/bin/bash
sudo su
cd /home/ec2-user
yum update -y
yum install wget unzip -y
wget https://packages.chef.io/files/stable/chef-workstation/24.4.1064/el/8/chef-workstation-24.4.1064-1.el8.x86_64.rpm
yum localinstall chef-workstation-24.4.1064-1.el8.x86_64.rpm -y
```

Verify installation:

```bash
chef --version
```

---

## 2. Chef Hosted Server & Starter Kit

1. Create a Chef account from Chef Hosted Server.
2. Download **Starter Kit** and upload to the workstation.
3. Upload your `.pem` file.
4. Unzip the starter kit:
   ```bash
   unzip chef-starter.zip
   ```
5. Navigate to:
   ```bash
   cd chef-repo/cookbooks
   ```

---

## 3. Standalone Usage (Local Development)

### Create a Cookbook

```bash
chef generate cookbook <YOUR_COOKBOOK_NAME>
```

Example:
```bash
chef generate cookbook olaeats
```

### Create a Recipe

Navigate to the recipe directory:

```bash
cd olaeats/recipes
vi install_tomcat.rb
```

Add:
```ruby
package 'tomcat'
```

Run locally:
```bash
chef-apply install_tomcat.rb
```

Verify:
```bash
which tomcat
```

---

### Uninstall Package

Modify recipe:

```ruby
package 'tomcat' do
  action :remove
end
```

Run:
```bash
chef-apply install_tomcat.rb
```

---

## 4. Multi-Package Installation

```ruby
package ['python', 'httpd', 'nano', 'vim']
```

---

## 5. Cluster Configuration (Node Management)

Worker Node AMI:
```
ami-029c0fbe456d58bd1
```

### Bootstrap Node

```bash
knife bootstrap <Worker_Node_Private_IP> --ssh-user ec2-user --sudo --identity-file /home/ec2-user/<Your_Pem_file> -N <Node_Name>
```

---

### Manage Run Lists

Add recipe:
```bash
knife node run_list add <nodename> 'recipe[<cookbookname>::<recipename>]'
```

Upload cookbook:
```bash
knife cookbook upload <cookbookname>
```

View status:
```bash
knife status --run-list
```

Remove recipe:
```bash
knife node run_list remove <nodename> 'recipe[<cookbookname>::<recipename>]'
```

---

### Force Node to Run

On Node:
```bash
chef-client
```

---

## 6. Summary Workflow

1. Create cookbook  
2. Write recipe  
3. Test locally  
4. Upload cookbook  
5. Update node run list  
6. Run chef-client  
