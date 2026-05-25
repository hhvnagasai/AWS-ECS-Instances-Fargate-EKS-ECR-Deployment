# AWS-ECS-Instances-Fargate-EKS-ECR-Deployment
# 
## Project Overview

This project demonstrates an enterprise-grade deployment of a React/Vite frontend application on Amazon ECS using both:

* AWS Fargate
* ECS Managed EC2 Instances

The application is containerized using Docker, stored in Amazon ECR, and deployed through ECS with Application Load Balancer integration.

---

# Architecture

```text
GitHub Repository
        ↓
Docker Build
        ↓
Amazon ECR
        ↓
Amazon ECS Cluster
   ├── Fargate Capacity
   └── Managed EC2 Capacity
        ↓
ECS Services & Tasks
        ↓
Application Load Balancer
        ↓
Public Users
```

---

# Technologies Used

| Technology                | Purpose                      |
| ------------------------- | ---------------------------- |
| React + Vite              | Frontend Application         |
| Docker                    | Containerization             |
| Amazon ECR                | Container Registry           |
| Amazon ECS                | Container Orchestration      |
| AWS Fargate               | Serverless Container Runtime |
| ECS Managed EC2           | Managed Compute Capacity     |
| Application Load Balancer | Traffic Routing              |
| CloudWatch                | Logging & Monitoring         |
| IAM                       | AWS Security & Permissions   |

---

# Repository Structure

```text
meghana-pro/
 ├── frontend/
 │    ├── src/
 │    ├── public/
 │    ├── package.json
 │    ├── vite.config.js
 │    ├── Dockerfile
 │    └── .dockerignore
 │
 └── backend/
```

---

# Frontend Environment Configuration

Create `.env` file inside frontend directory:

```env
VITE_API_URL=http://backend-service:5000/api/v1
```

---

# Docker Build Process

## Multi-Stage Docker Build

This project uses a production-grade multi-stage Docker build.

### Stage 1

* Node.js Builder
* Install dependencies
* Build React/Vite application

### Stage 2

* NGINX Runtime
* Serve optimized static files

---

# Dockerfile

```dockerfile
# ================================
# Stage 1 - Build React/Vite App
# ================================

FROM node:20 AS builder

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

RUN npm run build

# ================================
# Stage 2 - Production NGINX
# ================================

FROM nginx:alpine

RUN rm -rf /usr/share/nginx/html/*

COPY --from=builder /app/dist /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

---

# Build Docker Image

```bash
cd frontend

docker build -t meghana-frontend:v1 .
```

---

# Push Docker Image to Amazon ECR

## Create Repository

```bash
aws ecr create-repository \
--repository-name meghana-frontend \
--region ap-south-1
```

## Login to ECR

```bash
aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin <ACCOUNT_ID>.dkr.ecr.ap-south-1.amazonaws.com
```

## Tag Image

```bash
docker tag meghana-frontend:v1 <ACCOUNT_ID>.dkr.ecr.ap-south-1.amazonaws.com/meghana-frontend:v1
```

## Push Image

```bash
docker push <ACCOUNT_ID>.dkr.ecr.ap-south-1.amazonaws.com/meghana-frontend:v1
```

---

# ECS Deployment Models

## AWS Fargate

Serverless container deployment where AWS manages:

* Infrastructure
* Scaling backend
* Runtime isolation
* Host management

### Advantages

* No server management
* Fast deployment
* Better operational simplicity
* Excellent for microservices

---

## ECS Managed EC2

Managed ECS deployment using EC2 instances.

### Advantages

* SSH access
* More infrastructure control
* Better cost optimization
* Advanced customization

---

# ECS Cluster Setup

Cluster created with:

* AWS Fargate
* ECS Managed EC2 Instances
* Container Insights Enabled

---

# ECS Task Definition

Task Definition contains:

* Container image
* CPU allocation
* Memory allocation
* Port mappings
* Logging configuration

---

# ECS Service Features

The ECS Service provides:

* High availability
* Auto recovery
* Desired task management
* Load balancer integration
* Rolling deployments

---

# Application Load Balancer

The project uses ALB for:

* HTTP traffic routing
* Health checks
* Public access
* High availability

---

# CloudWatch Logging

Container logs are sent to:

```text
/ecs/meghana-frontend
```

This helps with:

* Troubleshooting
* Monitoring
* Observability
* Incident analysis

---

# Security Best Practices

This project follows enterprise DevOps security practices:

* IAM Roles instead of static credentials
* Private ECR repositories
* Security Groups for firewall management
* Multi-stage Docker builds
* Immutable container deployments

---

# Production Best Practices Implemented

* Multi-stage Docker builds
* Immutable image deployment
* ECS orchestration
* Hybrid compute strategy
* ALB integration
* CloudWatch monitoring
* IAM role-based authentication
* High availability deployment

---

# Learning Outcomes

This project demonstrates practical experience with:

* Docker containerization
* Amazon ECR
* Amazon ECS
* AWS Fargate
* ECS Managed EC2
* Application Load Balancer
* IAM roles and permissions
* Cloud-native deployment
* Enterprise DevOps workflows

---

# Future Enhancements

Potential improvements:

* Backend ECS deployment
* HTTPS using ACM
* Route53 custom domain
* Auto scaling policies
* CI/CD using Jenkins/GitHub Actions
* Terraform infrastructure automation
* ECS Service Discovery
* Blue-Green deployments

---

# Author - P.HARIHARA VENKATA NAGASAI

Developed as part of enterprise-grade DevOps and cloud-native deployment training on AWS.
