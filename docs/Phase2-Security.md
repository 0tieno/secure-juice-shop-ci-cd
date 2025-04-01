# Phase 2: Security Testing and analysis findings

## 1. Overview
In this phase, security testing was conducted using **OWASP ZAP, Trivy, and Snyk** to identify vulnerabilities in the system. The results are categorized based on severity to prioritize mitigation efforts.

## 2. Security Testing Tools
The following tools were used for vulnerability assessment:

- **[OWASP ZAP](https://www.zaproxy.org/)** – Automated and manual web application security testing.
- **[Trivy](https://aquasecurity.github.io/trivy/)** – Container image and dependency scanning.
- **[Snyk](https://snyk.io/)** – Security analysis for dependencies and Infrastructure as Code (IaC).

## 3. Summary of Findings
The vulnerabilities detected are categorized based on severity:

| Tool        | Vulnerability                         | Severity  | Affected Component |
|------------|--------------------------------------|----------|-------------------|
| OWASP ZAP  | SQL Injection                       | Critical | Web Application   |
| OWASP ZAP  | XSS (Cross-Site Scripting)          | High     | Web Application   |
| Trivy      | High-risk CVE in base image         | High     | Docker Container  |
| Snyk       | Outdated dependency with known RCE  | Medium   | Backend Service   |
| Trivy      | Misconfigured IAM role in IaC       | Low      | Kubernetes IaC    |

📌 **Screenshots of Findings**
_Include screenshots of detected vulnerabilities here for documentation purposes._

 OWASP ZAP Report
![OWASP ZAP Report](https://github.com/user-attachments/assets/dce0bd67-a634-4265-aa8c-bc5e6a7f2826)


OWASP ZAP SQL Injection Report
![OWASP ZAP SQL Injection Report](https://github.com/user-attachments/assets/3b3ce203-323d-4154-8cdc-09aa5d4586b9)


OWASP ZAP summaries Report
![OWASP ZAP summaries Report](https://github.com/user-attachments/assets/64b7b5aa-73fa-4ecd-8d01-c9fa8364b1cf)


Trivy Container Scan Report
![Trivy Container Scan Report](https://github.com/user-attachments/assets/48944396-4d72-4182-aa9d-10d6acb400d0)


Snyk Dependency Vulnerability
![Snyk Dependency Vulnerability](https://github.com/user-attachments/assets/ab9cf60c-6a2f-464d-98d5-9963e9bae6f6)

## 4. Detailed Analysis

### 4.1 OWASP ZAP Findings
- **SQL Injection**
  - Endpoint: `/login`
  - Risk: Critical
  - Description: Unvalidated user input in SQL queries.
  - Suggested Fix: Use **prepared statements** to prevent SQLi.

- **Cross-Site Scripting (XSS)**
  - Endpoint: `/search`
  - Risk: High
  - Description: JavaScript injection vulnerability.
  - Suggested Fix: Properly escape user input and use **Content Security Policy (CSP)**.

### 4.2 Trivy Findings
- **Container Image Vulnerabilities**
  - Affected Image: `node:14-alpine`
  - CVE: **CVE-2024-XXXXX**
  - Risk: High
  - Suggested Fix: Update to `node:16-alpine`.

- **Infrastructure as Code (IaC) Misconfiguration**
  - Affected File: `terraform/main.tf`
  - Issue: IAM role has **excessive permissions**.
  - Suggested Fix: Follow **least privilege principle**.

### 4.3 Snyk Findings
- **Dependency Vulnerabilities**
  - Affected Package: `express@4.17.1`
  - Risk: Medium
  - Suggested Fix: Upgrade to `express@4.18.2`.

## 5. Next Steps: Securing the CI/CD Pipeline
With these findings, the next phase will focus on **framework design and security integration** within the **CI/CD pipeline**:

1. **Implement fixes for critical vulnerabilities.**
2. **Integrate security scanning tools into the CI/CD workflow.**
3. **Define security policies for containerized deployments.**

---

📌 _End of Phase 2 Report._  
Proceeding to **Phase 3: Framework – Design and Architecture**.
