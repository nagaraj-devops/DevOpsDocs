# DevOps Training Notes (Dec 2025 – Jan 2026)

This document organizes DevOps training into environment setup, core concepts, and daily learning progression.

---

## 🛠️ Environment Setup & Prerequisites

### Required Software (Local Laptop)

| Category | Software | Version / Details |
|--------|----------|-------------------|
| DevOps Tools | Java (JDK) | 17.0.x (LTS) |
|  | Jenkins | Any stable version |
|  | Tomcat | 9.0.65 or 9.0.67 |
|  | Git & Maven | Git (Any), Maven 3.8.8 |
|  | Ngrok | Any version |
| Productivity | Editors | VS Code, Notepad++, MobaXterm |
| Communication | Tools | Brave Browser, WhatsApp Web |
| Accounts | Cloud | AWS, Azure, Google Cloud |
|  | Automation | GitHub, Terraform Cloud |

---

## 🏗️ Core DevOps Concepts

### Build Process (CI)
- IPL/IPO Model: Input → Process → Output
- Maven uses `pom.xml`
- Maven lifecycles include `package`
- Artifacts: `.war`, `.jar`, `.ear`, `.exe`

### Deployment & Infrastructure
- Tomcat as application server
- Infrastructure as Code (IaC)
- DSL using YAML for automation

---

## 📅 Daily Training Log

### Phase 1 – Jenkins & CI/CD

**Dec 12–15, 2025**
- One-click deployments
- Git → Jenkins → Maven
- Upstream/Downstream pipelines
- Environments: Dev → QA → UAT → Prod

**Dec 17, 2025**
- Master–Worker architecture
- SSH-based communication

---

### Phase 2 – Configuration Management

**Dec 22, 2025 – Chef**
- Ruby based
- Pull-based
- Cookbooks & Recipes

**Dec 23 – Jan 8 – Ansible**
- Push-based, Agentless
- YAML playbooks
- Handlers, Vault, Loops, Facts
- Ansible Tower

---

### Phase 3 – Infrastructure & Cloud

**Jan 9, 2026**
- Ansible Galaxy (geerlingguy.jenkins)
- Terraform (Multi-cloud IaC)

---

## 💻 Useful Commands

### SSH
```bash
ssh -i /path/to/key.pem ec2-user@<IP-ADDRESS>
```

### Chef Standalone
```bash
vi gitmgmt.rb
# Add: package 'git'
chef-apply gitmgmt.rb
```

### Ansible Playbook
```bash
ansible-playbook tomcat.yml
```

---

End of DevOps Training Notes
