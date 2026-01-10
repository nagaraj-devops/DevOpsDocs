
# CI/CD Pipeline using Jenkins, Maven, and Tomcat

This guide outlines the process of creating a complete CI/CD pipeline using Jenkins, Maven, and Tomcat.

## Phase 1: The Initial Build Job

### Step 1: Create 'Anatomy' Job
- Job Type: Freestyle project
- Repository: Fork https://github.com/nramnad/maven-project.git
- SCM: Git with your GitHub URL
- Build Step: Invoke top-level Maven targets
- Goal: package

### Step 2: Manual Execution
Click **Build Now** and verify workspace.

### Step 3: Quality Metrics
- Install Checkstyle plugin
- Maven Goal: `clean package checkstyle:checkstyle`
- Post-build: Publish checkstyle analysis results

## Phase 2: Artifact Management & Staging

### Step 4: Archive Artifacts
- Post-build: Archive artifacts
- Files: `**/*.war`

### Step 5: Package Job
- Copy from Anatomy job

### Step 5A: Deploy to Staging
- Copy artifacts from Package job
- Deploy WAR to Tomcat

### Step 5B: Job Chaining
- Package → deploy-to-staging

## Phase 3: Pipeline Visualization
- Install Build Pipeline plugin
- Initial job: Package

## Phase 4: Production Deployment
- Configure Tomcat on port 9090
- Create deploy-to-prod job
- Copy artifacts from package
- Deploy WAR
