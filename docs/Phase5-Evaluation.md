# Phase 5: Evaluating Effectiveness

## Goal
Measure how well the optimized pipeline balances security and speed.

## Steps

### 1. Compare Build Times Before and After Security Checks
- **"Before" Pipeline**: No security checks, only dependency updates, build, and push.
  - YAML: `azure-pipelines-before.yml`
  - Total Time: [e.g., 2m 15s] (from Azure DevOps logs).
- **"After" Pipeline**: Includes Trivy scanning.
  - YAML: `azure-pipelines.yml`
  - Total Time: [2m 45s] (from Azure DevOps logs).
  - Trivy Step Time: [30s - 60s] (from "Scan Image with Trivy" step).
- **Difference**: [30s] added by security checks.

### 2. Count Vulnerabilities Detected Before Deployment
- **Before**: 0 vulnerabilities (no scanning).
- **After**: 
  - Trivy detected 11 HIGH vulnerability (hardcoded private key in `juice-shop/lib/insecurity.ts`).
  - Full scan (optional): [e.g., 20 LOW, 10 MEDIUM, 1 HIGH] if run without severity filter.
- **Improvement**: Security scanning identifies issues pre-deployment.

### 3. Gather Insights from Logs and Reports
- **Logs**:
  - "Before": Successful build and push, no security visibility.
  - "After": Trivy logs show a HIGH-severity secret, proving security automation works.
- **Timing**: Trivy adds [30-60s], a minor delay for significant security gains.
- **ACR**: Images pushed as `juiceshopacr.azurecr.io/juice-shop:before` and `latest`.

## Results
- **Security Improved**: Trivy detects at least 1 HIGH vulnerability, vs. 0 in the "before" state.
- **Speed Impact**: Security checks add [a30s] to a [2m 45s] pipeline, a ~20% increase—acceptable for a PoC.
- **Outcome**: The pipeline balances security and speed effectively, meeting Phase 5 goals.

**Date Evaluated**: April 2, 2025  
**Live App**: [https://juice-shop-yr4-hsadgedxccamawcc.southafricanorth-01.azurewebsites.net](https://juice-shop-yr4-hsadgedxccamawcc.southafricanorth-01.azurewebsites.net)  
**ACR Images**: `juiceshopacr.azurecr.io/juice-shop:before`, `juiceshopacr.azurecr.io/juice-shop:latest`
