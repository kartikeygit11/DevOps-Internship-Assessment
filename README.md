## **📋 Project Overview** {#project-overview}

> This project implements a production-ready DevOps pipeline for a
> Next.js application with:

- Containerization using Docker with multi-stage builds

- CI/CD automation via GitHub Actions

- Container Registry using GitHub Container Registry (GHCR)

- Orchestration with Kubernetes manifests for Minikube deployment

## **🏗️ Architecture** {#architecture}

> text

Next.js App → Docker Container → GHCR Registry → Kubernetes Deployment

↓ ↓ ↓ ↓

React Multi-stage Automated Minikube

> Framework Builds CI/CD Cluster

## **📁 Repository Structure** {#repository-structure}

> text

DevOps-Internship-Assessment/

├── .github/workflows/

│ └── ci-cd.yml \# GitHub Actions CI/CD pipeline

├── k8s/

│ ├── deployment.yaml \# Kubernetes Deployment

│ └── service.yaml \# Kubernetes Service

├── src/app/

│ ├── layout.js \# Next.js layout

│ └── page.js \# Main application page

├── Dockerfile \# Multi-stage Docker build

├── .dockerignore \# Docker ignore rules

├── next.config.js \# Next.js configuration

├── package.json \# Node.js dependencies

├── package-lock.json \# Dependency lock file

> └── README.md \# This file

## **🚀 Quick Start** {#quick-start}

### **Prerequisites**

- Docker - For containerization

- Node.js 18+ - For local development

- Minikube - For local Kubernetes cluster

- kubectl - For Kubernetes management

### **Local Development**

1.  Clone the repository

2.  bash

git clone
https://github.com/kartikeygit11/DevOps-Internship-Assessment.git

3.  cd DevOps-Internship-Assessment

4.  Install dependencies

5.  bash

6.  npm install

7.  Run development server

8.  bash

9.  npm run dev  
    > Application will be available at http://localhost:3000

### **Docker Local Build**

1.  Build Docker image

2.  bash

3.  docker build -t nextjs-app .

4.  Run container

5.  bash

6.  docker run -p 3000:3000 nextjs-app  
    > Access at http://localhost:3000

## **🔧 Kubernetes Deployment with Minikube** {#kubernetes-deployment-with-minikube}

### **1. Start Minikube Cluster** {#start-minikube-cluster}

> bash

*\# Start Minikube with adequate resources*

minikube start \--memory=4096 \--cpus=2

*\# Enable Minikube\'s Docker daemon (for local testing)*

> eval \$(minikube docker-env)

### **2. Build and Deploy Application** {#build-and-deploy-application}

> bash

*\# Build image in Minikube environment*

docker build -t nextjs-app .

*\# Apply Kubernetes manifests*

kubectl apply -f k8s/

*\# Alternative: Apply individual resources*

kubectl apply -f k8s/deployment.yaml

> kubectl apply -f k8s/service.yaml

### **3. Verify Deployment** {#verify-deployment}

> bash

*\# Check all resources*

kubectl get all

*\# Check pods status*

kubectl get pods

*\# Check services*

kubectl get services

*\# Check deployment status*

kubectl get deployment

*\# View pod logs*

> kubectl logs -f deployment/nextjs-app

### **4. Access the Application** {#access-the-application}

> bash

*\# Get the Minikube service URL*

minikube service nextjs-service \--url

*\# Or use port-forward for direct access*

> kubectl port-forward service/nextjs-service 8080:80
>
> Access the application using the provided URL or at
> http://localhost:8080

## **🛠️ CI/CD Pipeline** {#cicd-pipeline}

> The GitHub Actions workflow automatically:

### **Build Pipeline**

- ✅ Triggered on push to main branch and pull requests

- ✅ Builds Docker image using multi-stage builds

- ✅ Pushes to GitHub Container Registry (GHCR)

- ✅ Tags images with branch and commit SHA

- ✅ Uses Buildx caching for faster builds

### **Image Tags**

- main - Latest build from main branch

- main-\<commit-sha\> - Specific commit builds

- pr-\<number\> - Pull request builds

### **GHCR Images**

- Main Image: ghcr.io/kartikeygit11/devops-internship-assessment:main

- Latest SHA:
  > ghcr.io/kartikeygit11/devops-internship-assessment:main-\<commit-sha\>

## **📦 Kubernetes Resources** {#kubernetes-resources}

### **Deployment (k8s/deployment.yaml)** {#deployment-k8sdeployment.yaml}

- Replicas: 3 instances for high availability

- Health Checks: Liveness and readiness probes

- Resource Limits: CPU and memory constraints

- Rolling Updates: Zero-downtime deployments

- Horizontal Pod Autoscaler: Automatic scaling (2-10 replicas)

### **Service (k8s/service.yaml)** {#service-k8sservice.yaml}

- Type: LoadBalancer for external access

- Port: 80 external → 3000 internal

- Selector: Matches app: nextjs-app labels

### **Health Monitoring**

> yaml

livenessProbe:

httpGet:

path: /

port: 3000

initialDelaySeconds: 10

periodSeconds: 5

readinessProbe:

httpGet:

path: /

port: 3000

initialDelaySeconds: 5

> periodSeconds: 5

## **🐳 Docker Optimization** {#docker-optimization}

> The Dockerfile implements best practices:

### **Multi-stage Build**

- Stage 1 (deps): Dependency installation

- Stage 2 (builder): Application build

- Stage 3 (runner): Production runtime

### **Security Features**

- Non-root user execution

- Minimal Alpine base image

- No development dependencies in production

- Proper file permissions

### **Performance Optimizations**

- Layer caching optimization

- .dockerignore to exclude unnecessary files

- Multi-architecture support ready

## **📊 Monitoring and Management** {#monitoring-and-management}

### **Useful Kubernetes Commands**

> bash

*\# Scale deployment*

kubectl scale deployment nextjs-app \--replicas=5

*\# View detailed pod information*

kubectl describe pod \<pod-name\>

*\# View resource usage*

kubectl top pods

*\# View events*

kubectl get events \--sort-by=.metadata.creationTimestamp

*\# Debugging*

> kubectl exec -it \<pod-name\> \-- /bin/sh

### **Application Logs**

> bash

*\# Stream logs from all pods*

kubectl logs -f -l app=nextjs-app

*\# View logs from specific pod*

kubectl logs -f \<pod-name\>

*\# View previous container logs*

> kubectl logs -f \<pod-name\> \--previous

## **🧹 Cleanup** {#cleanup}

### **Remove Kubernetes Resources**

> bash

*\# Delete all resources*

kubectl delete -f k8s/

*\# Delete specific resources*

kubectl delete deployment nextjs-app

> kubectl delete service nextjs-service

### **Stop Minikube**

> bash

*\# Stop Minikube cluster*

minikube stop

*\# Delete Minikube cluster (complete cleanup)*

> minikube delete

### **Clean Docker Images**

> bash

*\# Remove all unused containers, networks, and images*

docker system prune -a

*\# Remove specific image*

> docker rmi nextjs-app

## **🔍 Troubleshooting** {#troubleshooting}

### **Common Issues**

1.  Image Pull Errors

2.  bash

*\# Ensure Minikube Docker env is set*

3.  eval \$(minikube docker-env)

4.  Port Already in Use

5.  bash

*\# Kill process using port 3000*

6.  lsof -ti:3000 \| xargs kill -9

7.  Build Failures

8.  bash

*\# Clear Docker cache*

docker system prune -a

*\# Clear Next.js cache*

rm -rf .next node_modules

9.  npm install

10. Kubernetes Pod Issues

11. bash

*\# Check pod status and events*

kubectl describe pod \<pod-name\>

*\# Check cluster status*

12. minikube status

### **Debugging Commands**

> bash

*\# Check Minikube status*

minikube status

*\# Check Kubernetes nodes*

kubectl get nodes

*\# Check all resources in namespace*

kubectl get all

*\# View detailed service information*

> kubectl describe service nextjs-service



> Built with ❤️ using Next.js, Docker, GitHub Actions, and Kubernetes
