# 📌 Phase 1: Setting Up the CI/CD Environment

## **1️⃣ Introduction**

### **What is Phase 1 About?**

Phase 1 focuses on setting up the **CI/CD environment** to support secure DevOps practices. This step ensures we have an automated pipeline to build, test, and deploy applications, which will later integrate security checks.

### **Goal of Phase 1**

✅ Establish a working **CI/CD pipeline** using **Azure DevOps** and **GitHub** before adding security automation.
✅ Automate builds and tests for the **OWASP Juice Shop** vulnerable web application.

---

## **2️⃣ Tools and Technologies Used**

| Tool | Purpose |
|------|---------|
| **Azure DevOps** | CI/CD automation |
| **GitHub** | Version control |
| **OWASP Juice Shop** | Vulnerable application for security testing |
| **Node.js & npm** | Required to run Juice Shop |
| **YAML** | Used for configuring the Azure DevOps pipeline |

---

## **3️⃣ Step-by-Step Setup Process**

### **🔹 Step 1: Link GitHub with Azure DevOps**

1. Go to **Azure DevOps** ([dev.azure.com](https://dev.azure.com/))
2. Create a new project.
3. Navigate to **Project Settings → Service connections → New service connection → GitHub**.
4. Authenticate with GitHub and select your repository.
5. Click **Save**.

### **🔹 Step 2: Clone Juice Shop and Push to GitHub**

1. Open terminal or Git Bash.
2. Run the following commands:

   ```sh
   git clone https://github.com/juice-shop/juice-shop.git
   cd juice-shop
   git remote add origin <your-github-repo-url>
   git branch -M main
   git push -u origin main
   ```

3. Go to **GitHub** and confirm that Juice Shop is uploaded.

### **🔹 Step 3: Create an Azure DevOps Pipeline**

1. In **Azure DevOps**, go to **Pipelines → New pipeline**.
2. Select **GitHub** and choose your repository.
3. Choose **Starter Pipeline** and replace the YAML content with the following:

    ```yaml
    trigger:
    - main
    
    pool:
      vmImage: 'ubuntu-latest'
    
    steps:
    - task: NodeTool@0
      inputs:
        versionSpec: '18.x'
      displayName: 'Install Node.js'
    
    - script: |
        npm install
        npm test
      displayName: 'Install dependencies and run tests'
    ```

4. Click **Save and Run**.
5. Verify that the pipeline **executes successfully**.

---

## **4️⃣ Explanation of the CI/CD Pipeline (YAML File)**

| YAML Code | Explanation |
|-----------|------------|
| `trigger: - main` | Runs the pipeline every time you push code to the `main` branch. |
| `pool: vmImage: 'ubuntu-latest'` | Uses a Linux-based build agent. |
| `NodeTool@0` | Installs Node.js (needed for Juice Shop). |
| `npm install` | Installs dependencies for the application. |
| `npm test` | Runs automated tests to verify that Juice Shop is working. |

---

## **5️⃣ Challenges & Solutions**

| Issue | Solution |
|-------|----------|
| Authentication error when linking GitHub | Generated a new **Personal Access Token (PAT)** with the correct permissions. |
| Pipeline failed due to missing Node.js | Added the **NodeTool@0** task to install Node.js. |

---

## **6️⃣ Summary of Phase 1**

✅ **Successfully set up** Azure DevOps and GitHub.
✅ **Created and tested** a working CI/CD pipeline.
✅ **Automated build & test execution** for Juice Shop.

---

## **📂 Where to Document This?**

✅ **GitHub Repository** → Add this README.md file.
✅ **Final Project Report** → Include this documentation under "Phase 1: CI/CD Setup".

---

## **🎯 Why Is This Important?**

- 📌 Ensures **continuous integration** by automatically testing code changes.
- 🔍 Lays the foundation for adding **security automation** in later phases.
- 🚀 Helps with **faster development and deployment** in DevOps workflows.

---

## **📌 Next Steps (Phase 2 Preview)**

🔜 **Security Scanning** → Integrate security tools like **OWASP ZAP, SonarQube, and Trivy**.
🔜 **Security Gap Analysis** → Identify security weaknesses in the pipeline using **OWASP ZAP, Snyk, and Checkmarx**.
🔜 **Optimization** → Improve pipeline efficiency by **parallelizing tasks** and **reducing build times**.
