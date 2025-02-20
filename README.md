# Secure CI/CD Pipeline for OWASP Juice Shop - Final Year Project

## Project Title

“ A framework for balancing security and speed in CI/CD pipelines. A Shift-Left DevSecOps Approach.”

## Project Goals

This project aims to address these gaps by developing a security-centric CI/CD pipeline model that integrates security practices from the outset, following a "shift-left" strategy as proposed by the DevSecOps movement. By embedding automated security tests at each stage of the development process, the project seeks to provide real-time feedback to developers, reduce vulnerability remediation costs, and minimize security risks, resulting in a more efficient and secure CI/CD pipeline (Gartner, 2018; Swanson, 2021).

## Project Phases

The project is divided into five phases, each focusing on a specific aspect of the CI/CD pipeline:

1. [**Phase 1: Setup**](docs/Phase1-Setup.md) - Setting Up the CI/CD Environment
2. [**Phase 2: Security**](docs/Phase2-Security.md) - Implementing automated security testing in the CI/CD pipeline.
3. [**Phase 3: Framework**](docs/Phase3-Framework.md) - Designing a secure CI/CD pipeline framework.
4. [**Phase 4: Implementation**](docs/Phase4-Implementation.md) - Integrating security tools and practices into the CI/CD pipeline.
5. [**Phase 5: Evaluation**](docs/Phase5-Evaluation.md) - Evaluating the effectiveness of the secure CI/CD pipeline model.

## Folder Structure

The project is organized into the following folders:

```bash
    📦 secure-juice-shop-ci-cd
     ┣ 📂 .github/workflows      # GitHub Actions (if needed in future)
     ┣ 📂 azure-pipelines        # Azure DevOps pipeline YAML files
     ┣ 📂 docs                   # Documentation files
     ┃ ┣ 📜 Phase1-Setup.md      # Detailed setup for Phase 1
     ┃ ┣ 📜 Phase2-Security.md   # Security analysis findings
     ┃ ┣ 📜 Phase3-Framework.md  # Design of secure CI/CD pipeline
     ┃ ┣ 📜 Phase4-Implementation.md  # Implementation details
     ┃ ┣ 📜 Phase5-Evaluation.md # Performance evaluation results
     ┣ 📂 juice-shop             # Cloned OWASP Juice Shop project
     ┣ 📜 README.md              # Main project overview
     ┣ 📜 azure-pipelines.yml    # Azure DevOps pipeline config
     ┗ 📜 LICENSE                # (If needed)
```

## Getting Started

- Clone the repository:
  
  ```bash
  git clone https://github.com/your-username/secure-juice-shop-ci-cd.git

    cd secure-juice-shop-ci-cd
    ```

- Follow the instructions in the [Phase 1: Setup](docs/Phase1-Setup.md) document to set up the CI/CD environment.
- Proceed with the subsequent phases to implement and evaluate the security-centric CI/CD pipeline model.
- Refer to the [README.md](juice-shop/README.md) file in the `juice-shop` folder for more information on the OWASP Juice Shop project.
- For any questions or feedback, please raise an issue in the repository.
- 🛡️Happy coding! 🚀
