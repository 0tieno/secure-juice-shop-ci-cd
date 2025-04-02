# Phase 4: Implementing and Testing the Secure CI/CD Pipeline

## Goal
Apply security automation in an Azure DevOps pipeline to demonstrate a secure CI/CD process for the OWASP Juice Shop application.

## Overview
This phase focuses on creating a minimal, functional CI/CD pipeline that:
- Updates dependencies for the Juice Shop app.
- Builds a Docker container image.
- Integrates security scanning with Trivy to detect vulnerabilities.
- Pushes the secure image to Azure Container Registry (ACR) for deployment.

The pipeline is deployed in Azure DevOps, and the resulting image is stored in ACR (`juiceshopacr.azurecr.io/juice-shop:latest`), ready to update the live web app at [https://juice-shop-yr4-hsadgedxccamawcc.southafricanorth-01.azurewebsites.net](https://juice-shop-yr4-hsadgedxccamawcc.southafricanorth-01.azurewebsites.net).




## Steps

### 1. Add Security Tools to the Azure Pipeline
- **Dependency Management**: Used `npm` to update Node.js dependencies in `juice-shop/`.
- **Container Security**: Integrated Trivy to scan the Docker image for HIGH and CRITICAL vulnerabilities.
- **Note**: Static code analysis (e.g., SonarQube) was initially attempted but excluded for simplicity after configuration issues.

### 2. Run Security Checks in CI/CD
- **Pipeline Configuration**: Defined in `azure-pipelines.yml` at the root of the repository.
- **Security Check**: Trivy scans the built image and logs vulnerabilities (e.g., a hardcoded private key in `juice-shop/lib/insecurity.ts`).
- **Behavior**: Configured to log issues without failing the pipeline, allowing the PoC to complete despite Juice Shop’s intentional vulnerabilities.

### 3. Optimize Pipeline Execution
- **Speed Impact**: Trivy scanning adds approximately 30-60 seconds to the pipeline runtime (visible in Azure DevOps build logs).
- **Optimization**: No caching implemented yet; potential improvements include caching Node.js dependencies or the Trivy vulnerability database.

## Pipeline Configuration
The following YAML defines the pipeline in Azure DevOps:

```yaml
trigger:
  - main

pool:
  vmImage: 'ubuntu-latest'

steps:
  # Checkout the repository
  - checkout: self
    fetchDepth: 0

  # Install Node.js and update dependencies
  - script: |
      curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
      sudo apt-get install -y nodejs
      cd juice-shop
      npm install -g npm
      npm ci
      npm update
    displayName: 'Update Node.js Dependencies'

  # Build and tag Docker image
  - task: Docker@2
    displayName: 'Build and Tag Docker Image'
    inputs:
      containerRegistry: 'juiceshop-acr'
      repository: 'juice-shop'
      command: 'build'
      Dockerfile: 'juice-shop/Dockerfile'
      tags: 'latest'
      arguments: '--tag juiceshopacr.azurecr.io/juice-shop:latest'

  # Scan image with Trivy
  - script: |
      sudo apt-get update
      sudo apt-get install -y wget
      wget https://github.com/aquasecurity/trivy/releases/download/v0.50.1/trivy_0.50.1_Linux-64bit.deb
      sudo dpkg -i trivy_0.50.1_Linux-64bit.deb
      trivy image --severity HIGH,CRITICAL juiceshopacr.azurecr.io/juice-shop:latest
    displayName: 'Scan Image with Trivy'

  # Push image to ACR
  - task: Docker@2
    displayName: 'Push Docker Image to ACR'
    inputs:
      containerRegistry: 'juiceshop-acr'
      repository: 'juice-shop'
      command: 'push'
      tags: 'latest'
```

The pipeline fails when HIGH & CRITICAL vulnerabilities are detected in the application

![image](https://github.com/user-attachments/assets/bf8e1b81-bb15-403c-9109-ef8d6e76fcfe)

![image](https://github.com/user-attachments/assets/b07c1a6e-8c5f-428a-867b-11bceda172d6)


Scan Results for the failed pipeline:

![image](https://github.com/user-attachments/assets/64636e88-4992-4a86-a90d-9c393860ac04)


### SUCCESSFUL SECURITY OPTIMIZED PIPELINE - PROOF OF CONCEPT

![image](https://github.com/user-attachments/assets/ada9b678-0e7a-4b2b-afcb-bafe50550921)

