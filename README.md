# Terraform AKS Azure — Kubernetes Infrastructure as Code

## Overview
This project provisions a production-ready Kubernetes cluster on Microsoft Azure using Terraform. The entire infrastructure is defined as code — no manual portal clicks required.

## Architecture
```
Terraform → Azure Resource Group
         → Azure Container Registry (ACR)
         → Azure Kubernetes Service (AKS)
              └── Nginx Deployment
              └── LoadBalancer Service (Public IP)
```

## Tech Stack
- **Terraform** — Infrastructure as Code
- **Azure Kubernetes Service (AKS)** — Managed Kubernetes cluster
- **Azure Container Registry (ACR)** — Private container image registry
- **kubectl** — Kubernetes CLI for app deployment
- **Azure CLI** — Authentication and credential management

## Prerequisites
- [Terraform](https://developer.hashicorp.com/terraform/install) >= 1.0
- [Azure CLI](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- Azure subscription with Contributor access

## Project Structure
```
terraform-aks-azure/
├── main.tf           # Core infrastructure resources
├── providers.tf      # Azure provider configuration
├── variables.tf      # Input variable declarations
├── outputs.tf        # Output values after apply
├── terraform.tfvars  # Variable values (gitignored)
└── modules/          # Reusable modules (coming soon)
```

## Usage

### 1. Clone the repo
```bash
git clone https://github.com/ochubavictor/terraform-aks-azure.git
cd terraform-aks-azure
```

### 2. Create your tfvars file
```bash
cp terraform.tfvars.example terraform.tfvars
```
Edit `terraform.tfvars` with your values:
```hcl
subscription_id     = "your-azure-subscription-id"
location            = "eastus"
resource_group_name = "rg-aks-project"
cluster_name        = "aks-cluster"
node_count          = 1
node_vm_size        = "Standard_DC2s_v3"
acr_name            = "your-unique-acr-name"
```

### 3. Initialise Terraform
```bash
terraform init
```

### 4. Plan the infrastructure
```bash
terraform plan
```

### 5. Apply
```bash
terraform apply
```

### 6. Connect kubectl to the cluster
```bash
az aks get-credentials --resource-group rg-aks-project --name aks-cluster
kubectl get nodes
```

### 7. Deploy a test app
```bash
kubectl create deployment nginx --image=nginx
kubectl expose deployment nginx --port=80 --type=LoadBalancer
kubectl get service nginx --watch
```

### 8. Destroy when done
```bash
terraform destroy
```

## Key Concepts Demonstrated
- **Declarative IaC** — describe desired state, Terraform handles the rest
- **Dependency graph** — Terraform automatically resolves resource creation order
- **State management** — Terraform tracks all provisioned resources
- **Sensitive outputs** — kubeconfig marked sensitive, never exposed in logs
- **Gitignore best practices** — tfvars and state files excluded from version control

## Author
Victor Ochuba — Cloud & DevOps Engineer
[ochubavictor.com](https://ochubavictor.com) | [GitHub](https://github.com/ochubavictor)
