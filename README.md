# EKS GitOps with Terraform

This repository demonstrates infrastructure as code (IaC) and GitOps practices using AWS EKS, Terraform, ArgoCD, and GitHub Actions. It provides a production-ready infrastructure setup that follows cloud-native best practices.

## Architecture Overview

This project sets up:
- AWS EKS cluster with managed node groups
- VPC with public and private subnets across multiple availability zones
- ArgoCD for GitOps-based deployments
- GitHub Actions for CI/CD pipelines

## Prerequisites

- AWS CLI configured with appropriate credentials
- Terraform (>= 1.0.0)
- kubectl
- helm
- GitHub account and personal access token (for GitHub Actions)

## Repository Structure

```
├── terraform/          # Infrastructure as Code
│   ├── environments/   # Environment-specific configurations
│   └── modules/        # Reusable Terraform modules
├── kubernetes/         # Kubernetes manifests
│   ├── argocd/         # ArgoCD configuration
│   └── apps/           # Application manifests
└── scripts/            # Utility scripts
```

## Getting Started

1. Clone the repository:
   ```bash
   git clone https://github.com/ironcladgeek/eks-gitops-terraform.git
   cd eks-gitops-terraform
   ```

2. Initialize Terraform:
   ```bash
   cd terraform/environments/dev
   terraform init
   ```

3. Deploy the infrastructure:
   ```bash
   terraform plan
   terraform apply
   ```

4. Configure kubectl:
   ```bash
   aws eks update-kubeconfig --name cluster-name --region region-name
   ```

5. Install ArgoCD:
   ```bash
   kubectl create namespace argocd
   kubectl apply -n argocd -f kubernetes/argocd/install/install.yaml
   ```

## Infrastructure Components

### EKS Cluster
- Managed node groups with auto-scaling
- OIDC provider integration
- AWS Load Balancer Controller

### Networking
- VPC with public and private subnets
- NAT Gateways for private subnet connectivity
- Security groups for cluster components

### Security
- IAM roles for service accounts (IRSA)
- Node groups with minimum required permissions
- Network policies for pod-to-pod communication

## GitOps Workflow

1. Infrastructure changes:
   - Make changes to Terraform code
   - Create a pull request
   - GitHub Actions will run Terraform plan
   - After approval and merge, changes will be applied

2. Application deployments:
   - Update Kubernetes manifests in `/kubernetes/apps`
   - ArgoCD will automatically sync changes to the cluster
   - Monitor deployment status in ArgoCD dashboard


