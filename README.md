# Cloud-Native Security Demo Project

A comprehensive repository demonstrating a cloud-native application with intentional security weaknesses in infrastructure-as-code (IaC), containerization, and CI/CD pipelines to showcase detection and remediation capabilities of security tools like Wiz, Microsoft Defender for Cloud, and GitHub Advanced Security.

## Overview

This repository serves as a playground for building and deploying a cloud-native application on Azure with deliberately introduced security issues. These include outdated and vulnerable Linux versions, publicly accessible storage accounts, exposed virtual machines, and databases without proper access controls. It provides the foundation for security scanning and analysis across infrastructure, containers, Kubernetes, and CI/CD pipelines, allowing you to discover and remediate issues using modern cloud security tools.

## Purpose

The primary goals of this project are to:

1. **Demonstrate cloud-native architecture** with typical real-world components
2. **Implement infrastructure-as-code** using Terraform and Azure
3. **Showcase containerization** with Docker and Azure Container Registry
4. **Deploy to Kubernetes** using Azure Kubernetes Service
5. **Automate with CI/CD** using GitHub Actions workflows

## Repository Structure

```
CloudNative-Security/
├── 01_terraform/          # Terraform configuration files
├── 02_kubernetes/         # Kubernetes manifests for deployment
├── 03_application/        # Application source code
│   ├── backend/           # Node.js backend API
│   └── web-app/           # React frontend application
├── .github/workflows/     # GitHub Actions CI/CD workflows
└── README.md              # This file
```

## Getting Started

### Prerequisites

- Azure Subscription
- GitHub Account
- Terraform CLI (v1.0+)
- Azure CLI
- kubectl
- Docker
- Node.js and npm

### Setup Instructions

1. **Clone the repository**

   ```bash
   git clone https://github.com/yourusername/wiz-security.git
   cd CloudNative-Security
   ```

2. **Set up Azure authentication**

   ```bash
   az login
   az account set --subscription <your-subscription-id>
   ```

3. **Configure GitHub Repository Secrets and Variables**

   Set up the following secrets in your GitHub repository:
   - `AZURE_CLIENT_ID`
   - `AZURE_CLIENT_SECRET`
   - `AZURE_TENANT_ID`
   - `AZURE_SUBSCRIPTION_ID`

   Set up the following variable as repository variable in your GitHub repository:
   - `RESOURCE_GROUP`

4. **Deploy with the CI/CD pipeline**

   Push changes to the `main` branch or manually trigger the ```End-to-End Deployment``` workflow from the GitHub Actions tab.

---

### Manual Deployment

#### Infrastructure

```bash
cd 01_terraform
terraform init
terraform plan -var "subscription_id=<your-subscription-id>" -var "environment=wizdemo"
terraform apply
```

#### Build and Push Docker Images

```bash
# Build backend
cd 03_application/backend
az acr build --registry <your-acr-name> --image backend-app:v1 .

# Build frontend
cd ../web-app
az acr build --registry <your-acr-name> --image web-app:v1 .
```

#### Deploy to Kubernetes

```bash
cd 02_kubernetes
az aks get-credentials --resource-group <your-resource-group> --name <your-aks-cluster>
kubectl apply -f deployment.yaml
kubectl apply -f service.yml
```

## CI/CD Pipeline

This repository includes GitHub Actions workflows that handle:

1. Building and pushing Docker images to ACR
2. Deploying infrastructure with Terraform
3. Deploying applications to Kubernetes

### Workflow Structure

- **build-images.yml**: Handles building and pushing Docker images to ACR
- **terraform-deploy.yml**: Manages infrastructure deployment using Terraform
- **kubernetes-deploy.yml**: Deploys applications to the Kubernetes cluster
- **end-to-end.yml**: Orchestrates the entire deployment pipeline

## Web Application Access

After successful deployment, access your web application at the external IP provided by the LoadBalancer service:

```bash
kubectl get service web-service -o jsonpath="{.status.loadBalancer.ingress[0].ip}"
```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the LICENSE file for details.

---

*Note: This project is intended for educational and demonstration purposes only.*