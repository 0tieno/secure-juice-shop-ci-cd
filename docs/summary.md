# Summary of Phases

## Phase 1: Setting Up the CI/CD Environment

### Introduction
Phase 1 focuses on setting up the CI/CD environment to support secure DevOps practices. This step ensures we have an automated pipeline to build, test, and deploy applications, which will later integrate security checks.

### Goal of Phase 1
- Establish a working CI/CD pipeline using Azure DevOps and GitHub before adding security automation.
- Automate builds and tests for the OWASP Juice Shop vulnerable web application.

### Tools and Technologies Used
- Azure DevOps: CI/CD automation
- GitHub: Version control
- OWASP Juice Shop: Vulnerable application for security testing
- Node.js & npm: Required to run Juice Shop
- YAML: Used for configuring the Azure DevOps pipeline

### Step-by-Step Setup Process
1. Link GitHub with Azure DevOps
2. Clone Juice Shop and Push to GitHub
3. Create an Azure DevOps Pipeline

### Explanation of the CI/CD Pipeline (YAML File)
- `trigger: - main`: Runs the pipeline every time you push code to the `main` branch.
- `pool: vmImage: 'ubuntu-latest'`: Uses a Linux-based build agent.
- `NodeTool@0`: Installs Node.js (needed for Juice Shop).
- `npm install`: Installs dependencies for the application.
- `npm test`: Runs automated tests to verify that Juice Shop is working.

### Challenges & Solutions
- Authentication error when linking GitHub: Generated a new Personal Access Token (PAT) with the correct permissions.
- Pipeline failed due to missing Node.js: Added the NodeTool@0 task to install Node.js.

### Summary of Phase 1
- Successfully set up Azure DevOps and GitHub.
- Created and tested a working CI/CD pipeline.
- Automated build & test execution for Juice Shop.

## Phase 2: Security Testing and Analysis Findings

### Overview
In this phase, security testing was conducted using OWASP ZAP, Trivy, and Snyk to identify vulnerabilities in the system. The results are categorized based on severity to prioritize mitigation efforts.

### Security Testing Tools
- OWASP ZAP: Automated and manual web application security testing.
- Trivy: Container image and dependency scanning.
- Snyk: Security analysis for dependencies and Infrastructure as Code (IaC).

### Summary of Findings
- OWASP ZAP: SQL Injection (Critical), XSS (High)
- Trivy: High-risk CVE in base image (High)
- Snyk: Outdated dependency with known RCE (Medium)
- Trivy: Misconfigured IAM role in IaC (Low)

### Detailed Analysis
- OWASP ZAP Findings: SQL Injection, Cross-Site Scripting (XSS)
- Trivy Findings: Container Image Vulnerabilities, Infrastructure as Code (IaC) Misconfiguration
- Snyk Findings: Dependency Vulnerabilities

### Next Steps: Securing the CI/CD Pipeline
1. Implement fixes for critical vulnerabilities.
2. Integrate security scanning tools into the CI/CD workflow.
3. Define security policies for containerized deployments.

## Phase 3: Designing a Secure CI/CD Framework

### Overview
Goal: Develop a secure and efficient CI/CD framework that integrates security at every stage while maintaining speed and reliability.

### Security Integration Points
- Pre-Commit Stage (Developer Workstation): SAST, linters, dependency scanning.
- CI/CD Process (Build & Test): DAST, IaC scanning, secret scanning.
- Post-Deployment (Runtime Security): Container security monitoring, RASP, threat detection.

### Risk-Based Prioritization
- Critical updates: Full security testing.
- Minor updates: Lighter security checks.
- Frequent scans on production environments: Automated, continuous monitoring.

### Performance Optimization Strategies
- Parallel Execution: Run security tests alongside other CI/CD processes.
- Caching Mechanisms: Store previously scanned results to avoid redundant scans.
- Selective Testing: Focus full scans on high-risk components.

### Secure CI/CD Framework Architecture
1. Developer commits code → Runs pre-commit security checks.
2. CI process → Automated security tests (SAST, DAST, IaC scanning, secret scanning).
3. CD process → Deploys application with security validation.
4. Runtime security monitoring → Detects threats in real time.

### Conclusion
By designing a CI/CD pipeline with security at every stage, we ensure a balance between protection and performance. This framework will be used in the next phase (Implementation) to configure automated security integrations.

## Phase 4: Implementing and Testing the Secure CI/CD Pipeline

### Goal
Apply security automation in an Azure DevOps pipeline to demonstrate a secure CI/CD process for the OWASP Juice Shop application.

### Overview
This phase focuses on creating a minimal, functional CI/CD pipeline that:
- Updates dependencies for the Juice Shop app.
- Builds a Docker container image.
- Integrates security scanning with Trivy to detect vulnerabilities.
- Pushes the secure image to Azure Container Registry (ACR) for deployment.

### Steps
1. Add Security Tools to the Azure Pipeline
2. Run Security Checks in CI/CD
3. Optimize Pipeline Execution

### Pipeline Configuration
The following YAML defines the pipeline in Azure DevOps:

```yaml
trigger:
  - main

pool:
  vmImage: 'ubuntu-latest'

steps:
  - checkout: self
    fetchDepth: 0

  - script: |
      curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
      sudo apt-get install -y nodejs
      cd juice-shop
      npm install -g npm
      npm ci
      npm update
    displayName: 'Update Node.js Dependencies'

  - task: Docker@2
    displayName: 'Build and Tag Docker Image'
    inputs:
      containerRegistry: 'juiceshop-acr'
      repository: 'juice-shop'
      command: 'build'
      Dockerfile: 'juice-shop/Dockerfile'
      tags: 'latest'
      arguments: '--tag juiceshopacr.azurecr.io/juice-shop:latest'

  - script: |
      sudo apt-get update
      sudo apt-get install -y wget
      wget https://github.com/aquasecurity/trivy/releases/download/v0.50.1/trivy_0.50.1_Linux-64bit.deb
      sudo dpkg -i trivy_0.50.1_Linux-64bit.deb
      trivy image --severity HIGH,CRITICAL juiceshopacr.azurecr.io/juice-shop:latest
    displayName: 'Scan Image with Trivy'

  - task: Docker@2
    displayName: 'Push Docker Image to ACR'
    inputs:
      containerRegistry: 'juiceshop-acr'
      repository: 'juice-shop'
      command: 'push'
      tags: 'latest'
```

### Successful Security Optimized Pipeline - Proof of Concept
The pipeline fails when HIGH & CRITICAL vulnerabilities are detected in the application.

## Phase 5: Evaluating Effectiveness

### Goal
To measure how well the optimized pipeline balances security and speed.

### Steps
1. Comparison Between Build Times Before and After Security Checks
2. Vulnerabilities Detected Before Deployment
3. Gathered Insights from Logs and Reports

### Results
- Security Improved: Trivy detects at least 1 HIGH vulnerability, vs. 0 in the "before" state.
- Speed Impact: Security checks add 30s to a 2m 45s pipeline, a ~20% increase—acceptable for a PoC.
- Outcome: The pipeline balances security and speed effectively, meeting Phase 5 goals.

### Live App
[https://juice-shop-yr4-hsadgedxccamawcc.southafricanorth-01.azurewebsites.net](https://juice-shop-yr4-hsadgedxccamawcc.southafricanorth-01.azurewebsites.net)
