
# Jenkins CI/CD Pipeline using Maven and Tomcat

This guide describes how to create a complete CI/CD pipeline using **Jenkins**, **Maven**, and **Tomcat**  
Repository used: https://github.com/nramnad/maven-project.git

---

## Step 1: Create First Job – *anatomy* (Freestyle)

1. Create a **New Item** in Jenkins.
2. Job name: **anatomy**
3. Select **Freestyle project** → Click **OK**.

### SCM Configuration
- Select **Git**
- Repository URL:
  ```
  https://github.com/nramnad/maven-project.git
  ```

### Build Configuration
- Add build step → **Invoke top-level Maven targets**
- Goals:
  ```
  package
  ```

4. Click **Save**.

---

## Step 2: Run Job Manually

1. Click **Build Now**
2. After build completion:
   - Go to **Workspace**
   - Verify Maven project directory structure is created

---

## Step 3: (Optional) Code Quality – Checkstyle

> *This step can be skipped if not required*

### Install Plugin
- Install **Checkstyle Plugin** from *Manage Jenkins → Plugins*

### Job Configuration
- Build Goals:
  ```
  clean package checkstyle:checkstyle
  ```

### Post-Build Action
- Select **Publish Checkstyle analysis results**

### Verify
- Run the job
- On the build page, click **Checkstyle Warnings**
- View Checkstyle report

---

## Step 4: Archive Generated Artifacts

### Configure Post-Build Action
- Select **Archive the artifacts**
- Files to archive:
  ```
  **/*.war
  ```

### Verify
- Run the job
- Jenkins archives `webapp.war`
- Check console output for artifact archiving logs

---

## Step 5: Create *package* Job

1. Create a new job named **package**
2. Select **Copy from existing job**
3. Source job: **anatomy**
4. Click **Save**

---

## Step 5A: Deploy to Staging

### Create Job: *deploy-to-staging*
- Job type: **Freestyle project**

### Build Section
- Select **Copy artifacts from other project**
- Project name: **package**
- Artifacts to copy:
  ```
  **/*.war
  ```

> Install **Copy Artifact Plugin** if not available

### Post-Build Action
- Select **Deploy war/ear to a container**
- WAR/EAR files:
  ```
  **/*.war
  ```
- Container: **Tomcat 7 / 8 / 9**
- Provide:
  - Tomcat Username
  - Tomcat Password
  - Tomcat URL (localhost or public DNS)

Click **Save**.

---

## Step 5B: Chain Jobs Automatically

### Configure *package* Job
- Post-Build Action → **Build other projects**
- Downstream project:
  ```
  deploy-to-staging
  ```

### Execute
1. Build **package**
2. On success, **deploy-to-staging** triggers automatically

### Verify
- Open browser:
  ```
  http://<tomcat-host>:<port>
  ```
- Application loads successfully

---

## Step 6: Jenkins Build Pipeline View

### Install Plugin
- Install **Build Pipeline Plugin**

### Create Pipeline View
1. Jenkins Dashboard → **+**
2. View name: **build-pipeline**
3. Select **Build Pipeline View**
4. Initial job:
   ```
   package
   ```
5. Save

### Result
- Visual pipeline showing upstream and downstream jobs
- Click **Run** to refresh pipeline

---

## Step 7: Parallel Jenkins Job – Static Analysis

### Create Job: *static-analysis*
- Job type: **Freestyle**
- SCM:
  ```
  https://github.com/nramnad/maven-project.git
  ```

### Build
- Invoke top-level Maven targets
- Goals:
  ```
  package
  ```

### Trigger in Parallel
- Go to **package** job configuration
- Post-Build Action → **Build other projects**
- Projects:
  ```
  static-analysis, deploy-to-staging
  ```

### Verify
- Build Pipeline view shows parallel execution

---

## Step 8: Deploy to Production

### Tomcat Configuration
- Edit `server.xml`
- Add new connector on port **9090**
- Restart Tomcat

### Create Job: *deploy-to-prod*

#### Build Section
- Copy artifacts from other project:
  ```
  package
  ```
- Artifacts:
  ```
  **/*.war
  ```

#### Post-Build Action
- Deploy war/ear to container
- Container: **Tomcat 7 / 8 / 9**
- Provide credentials and Tomcat URL (port 9090)

Click **Save**.

### Chain from Staging
- Go to **deploy-to-staging** job
- Post-Build Action → **Build other projects**
- Downstream:
  ```
  deploy-to-prod
  ```

---

## Final Result

- CI/CD pipeline with:
  - Build
  - Artifact packaging
  - Staging deployment
  - Parallel static analysis
  - Production deployment
- Visualized using **Build Pipeline View**

---

✅ End of Documentation
