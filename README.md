

# 🚀 Cloud-Native Full Stack Application (React + Node.js) with EKS & Terraform

This project is a production-grade, end-to-end cloud-native application setup using:

* **Node.js** for the backend
* **React.js** for the frontend
* **Docker** for containerization
* **Kubernetes (EKS)** for orchestration
* **Terraform** for provisioning AWS infrastructure
* **GitHub Actions** for CI/CD automation

✅ The infrastructure is completely configured and ready.
❗️To run the app, you'll need to set a few key environment variables (listed below).

---

## 📁 Project Structure Overview

```
.
├── .github/workflows/deploy.yml         # GitHub Actions pipeline for CI/CD
├── application
│   ├── backend                          # Node.js backend API
│   ├── frontend                         # React frontend app
│   └── k8s                              # Kubernetes manifests
├── bucket                               # Terraform config for S3 state bucket
├── terraform-eks                        # Terraform config for EKS cluster
├── docker-compose.yml                   # Local dev setup
├── .gitignore
└── README.md                            # You're here!
```

---

## 🔧 Required Environment Variables

Before deploying or running the pipeline, ensure the following environment variables are set:

| Variable                | Description                               |
| ----------------------- | ----------------------------------------- |
| `AWS_ACCESS_KEY_ID`     | Your AWS access key                       |
| `AWS_SECRET_ACCESS_KEY` | Your AWS secret key                       |
| `AWS_REGION`            | AWS region (e.g., `us-east-1`)            |
| `AWS_ACCOUNT_ID`        | Your AWS account ID                       |
| `CLUSTER_NAME`          | EKS cluster name                          |
| `ECR_REPO_NAME`         | ECR repo name to push/pull Docker images  |
| `TF_BUCKET_NAME`        | S3 bucket name for Terraform remote state (Give some unique name) |
                            
---

## 🚧 Infrastructure Setup (Already Done)

Terraform configuration is ready under:

* `terraform-eks/` – Provisions the EKS cluster and networking
* `bucket/` – (Optional) Provisions the S3 bucket for remote state

To re-run or re-init:

```bash
cd terraform-eks
terraform init
terraform apply
```

---

## 🐳 Docker Build (Manual or CI/CD)

You can manually build and push images:

```bash
# Backend
docker build -t $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/$ECR_REPO_NAME-backend ./application/backend
docker push $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/$ECR_REPO_NAME-backend

# Frontend
docker build -t $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/$ECR_REPO_NAME-frontend ./application/frontend
docker push $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/$ECR_REPO_NAME-frontend
```

---

## ☸️ Kubernetes Deployment

Kubernetes manifests are already defined inside `application/k8s/`. Once the images are pushed and your kubeconfig is set up, deploy with:

```bash
kubectl apply -f application/k8s/backend-deployment.yaml
kubectl apply -f application/k8s/frontend-deployment.yaml
kubectl apply -f application/k8s/ingress.yaml
```

---

## 🔁 GitHub Actions CI/CD (Optional)

The GitHub Actions workflow in `.github/workflows/deploy.yml` can:

* Build Docker images
* Push to ECR
* Deploy to EKS

Make sure to add the required secrets in your GitHub repository settings:

| Secret Name             |
| ----------------------- |
| `AWS_ACCESS_KEY_ID`     |
| `AWS_SECRET_ACCESS_KEY` |
| `AWS_REGION`            |
| `AWS_ACCOUNT_ID`        |
| `CLUSTER_NAME`          |
| `ECR_REPO_NAME`         |
| `TF_BUCKET_NAME`        |

---

## 🧪 Local Development (Optional)

To run locally:

```bash
cd application
docker-compose up --build
```

* Application DNS: After Deployment we can receive output.

---

## 📦 Cleanup

To destroy the cloud infrastructure:

```bash
cd terraform-eks
terraform destroy
```

---

## ✅ Final Checklist

* Infrastructure is provisioned (VPC, EKS, IAM, etc.)
* Kubernetes manifests are defined
* CI/CD configured through Github Actions
* Docker images built and pushed to ECR
* Kubernetes deployments applied
* App running on AWS EKS
* lb url dns will come as Output. we need to change region as per Github secret. [Eg.,"k8s-default-1935542352.<REGION>.elb.amazonaws.com"]

---
