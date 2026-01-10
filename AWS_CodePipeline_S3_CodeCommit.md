# AWS CodePipeline with S3 & CodeCommit

This guide explains how to build a simple CI/CD pipeline using **AWS S3, CodePipeline, CodeCommit, and SNS**.

---

## Architecture

- Source: **S3 or CodeCommit**
- Pipeline: **AWS CodePipeline**
- Deployment Target: **S3 Static Website**
- Notifications: **SNS (SMS)**

---

## Part 1 — Using S3 as Source

### Step 1: Prepare Website Files

Create a folder and download:

- index.html  
- error.html  

Zip them as:

```
my-website.zip
```

---

### Step 2: Create Source S3 Bucket

- Bucket name: `my-portal-source`
- Region: `us-east-1`
- Enable **Versioning**
- Upload `my-website.zip`

---

### Step 3: Create Production S3 Bucket

- Bucket name: `my-portal-prod`
- Object ownership: **ACLs enabled**
- Block Public Access: **Disabled**
- Versioning: **Disabled**

---

### Step 4: Enable Static Website Hosting

In `my-portal-prod` → **Properties**

- Enable Static Website Hosting  
- Index document: `index.html`  
- Error document: `error.html`

Save and note the website URL.

---

### Step 5: Create AWS CodePipeline

- Pipeline name: any name  
- Service Role: **New Service Role**

---

### Step 6: Source Stage

- Provider: **Amazon S3**
- Bucket: `my-portal-source`
- Object key: `my-website.zip`
- Change detection: **CloudWatch Events**

---

### Step 7: Build Stage

- Skip build stage

---

### Step 8: Deploy Stage

- Provider: **Amazon S3**
- Bucket: `my-portal-prod`
- Enable **Extract before deploy**
- Canned ACL: `public-read`

---

### Step 9: Verify Deployment

Open S3 static website URL — your website is live 🎉

---

# Part 2 — AWS CodeCommit + CodePipeline

---

## Step 1: Create IAM User

- Create IAM user (do NOT use root)
- Enable:
  - AWS CodeCommit
  - AWS CodePipeline
  - AWS SNS

---

## Step 2: Create CodeCommit Repository

- Add:
  - index.html
  - error.html
- Push files using Git

---

## Step 3: Create SNS Topic

- Topic name: `notify-for-codecommit`

---

## Step 4: Create Subscription

- Protocol: SMS
- Endpoint: `+91XXXXXXXXXX`
- Confirm SMS

---

## Step 5: Configure CodeCommit Notifications

Go to CodeCommit → Repository → Settings → Notifications

- Create Notification Rule  
- Select all events  
- Target: SNS topic  

---

## Step 6: Create CodeCommit Trigger

- Trigger name: `code-commit-sms`
- Events: All repository events
- Branch: `main`
- Target: SNS

---

## Result

Every Git push will:
- Trigger CodePipeline
- Deploy website
- Send SMS notification

---

End of AWS CodePipeline Documentation
