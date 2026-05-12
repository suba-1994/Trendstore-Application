**TrendStore DevOps Project**

**Project Overview**

This project demonstrates a complete end-to-end DevOps implementation for deploying a production-ready web application using modern DevOps tools and cloud technologies.

The application was containerized using Docker, automated through Jenkins CI/CD pipelines, deployed on AWS EKS Kubernetes cluster, and monitored using Prometheus and Grafana.

The goal of this project was to understand real-world DevOps workflows including:
----------------------------------------
Infrastructure provisioning
Containerization
CI/CD automation
Kubernetes deployment
Monitoring and observability
Cloud deployment using AWS
Technologies Used
Tool	Purpose
GitHub	Source code management
Docker	Containerization
DockerHub	Image repository
Jenkins	CI/CD automation
Terraform	Infrastructure as Code
AWS EC2	Jenkins server hosting
AWS EKS	Kubernetes cluster
Kubernetes	Container orchestration
Helm	Kubernetes package manager
Prometheus	Monitoring
Grafana	Visualization dashboards
-------------------------------------------

**Project Architecture
GitHub
   ↓
GitHub Webhook
   ↓
Jenkins CI/CD Pipeline
   ↓
Docker Build
   ↓
DockerHub Push
   ↓
Kubernetes Deployment
   ↓
AWS EKS Cluster
   ↓
Prometheus Monitoring
   ↓
Grafana Dashboard**
----------------------------------------------
GitHub Repository

Repository URL:

**https://github.com/suba-1994/Trendstore-Application**
----------------------------------------------------------
DockerHub Repository:

https://hub.docker.com/r/subashreedocker/trend-app
------------------------------------------------------------
Application Deployment URL

**http://aa3561776a06343fab5ce0847f0aef8e-1110357897.us-east-1.elb.amazonaws.com**
-------------------------------------------------------------
Grafana Monitoring URL
****
**http://a4effafcab1df4086803255b06f9128d-885645994.us-east-1.elb.amazonaws.com**
----------------------------------------------------------------
Step-by-Step Implementation

1. Docker Setup

Docker was installed and configured to containerize the application.

Dockerfile
FROM nginx:latest

COPY dist/ /usr/share/nginx/html

EXPOSE 80
Docker Build
docker build -t subashreedocker/trend-app:v1 .
Docker Run
docker run -d -p 8080:80 subashreedocker/trend-app:v1
---------------------------------------------------------------------------
2. DockerHub Integration

Docker image was pushed to DockerHub for centralized image management.

Docker Login
docker login
Push Image
docker push subashreedocker/trend-app:v1
----------------------------------------------------------------------------
3. Terraform Infrastructure Setup

Terraform was used to provision AWS EC2 infrastructure.

Terraform Initialization
terraform init
Terraform Plan
terraform plan
Terraform Apply
terraform apply

Resources created:

Jenkins EC2 instance
Security groups
AWS networking components
-----------------------------------------------------------------------------------
4. Jenkins CI/CD Pipeline

Jenkins was installed on AWS EC2 instance and configured for CI/CD automation.

Jenkins Pipeline Stages
Clone Repository
Build Docker Image
DockerHub Authentication
Push Docker Image
Deploy to Kubernetes

Jenkinsfile
pipeline {
    agent any

    environment {
        IMAGE_NAME = "subashreedocker/trend-app"
        IMAGE_TAG = "v1"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git 'https://github.com/suba-1994/Trendstore-Application.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
            }
        }

        stage('DockerHub Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push $IMAGE_NAME:$IMAGE_TAG'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }
    }
}
--------------------------------------------------------------------
**5. GitHub Webhook Integration**

GitHub webhook was configured to automatically trigger Jenkins pipeline whenever code changes are pushed to the repository.

Webhook URL:

**http://<JENKINS_PUBLIC_IP>:8080/github-webhook/**
--------------------------------------------------------------------------
**6. AWS EKS Kubernetes Deployment**

An AWS EKS cluster was created using eksctl.

EKS Cluster Creation
eksctl create cluster \
--name trend-cluster \
--region us-east-1 \
--nodegroup-name workers \
--node-type t3.small \
--nodes 1

Kubernetes Deployment Configuration

deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: trend-app

spec:
  replicas: 1

  selector:
    matchLabels:
      app: trend-app

  template:
    metadata:
      labels:
        app: trend-app

    spec:
      containers:
      - name: trend-app
        image: subashreedocker/trend-app:v1

        ports:
        - containerPort: 80
service.yaml
apiVersion: v1
kind: Service

metadata:
  name: trend-app-service

spec:
  type: LoadBalancer

  selector:
    app: trend-app

  ports:
  - port: 80
    targetPort: 80
---------------------------------------------------------
Kubernetes Commands Used
Deploy Application
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
Check Pods
kubectl get pods
Check Services
kubectl get svc
Check Nodes
kubectl get nodes
-----------------------------------------------------
**7. Monitoring Setup**

Monitoring was implemented using Prometheus and Grafana.

Helm Repository Setup
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

helm repo add grafana https://grafana.github.io/helm-charts

**Prometheus Installation**
helm install prometheus prometheus-community/prometheus \
-n monitoring --create-namespace

**Grafana Installation**
helm install grafana grafana/grafana \
-n monitoring \
--set service.type=LoadBalancer
Monitoring Components
Component	Purpose
Prometheus	Metrics collection
Grafana	Visualization dashboards
Node Exporter	Node metrics
Kube State Metrics	Kubernetes metrics
Challenges Faced During Project

During implementation, several real-world DevOps issues were encountered and resolved:

Docker installation issues in Git Bash
Terraform IAM permission errors
Jenkins Java compatibility issues
Kubernetes image pull errors
AWS EKS permission configuration
Prometheus resource limitations
Grafana datasource connectivity issues

These issues helped in understanding practical troubleshooting in DevOps environments.
-------------------------------------------------------------------

Key Learnings

Through this project, I gained hands-on experience in:

CI/CD pipeline creation
Infrastructure provisioning using Terraform
Docker image management
Kubernetes deployments
AWS EKS management
Monitoring and observability
Jenkins automation
GitHub webhook integration
----------------------------------------------------------------------
Final Outcome

Successfully built and deployed a complete DevOps pipeline with automated CI/CD workflow, Kubernetes deployment, and monitoring infrastructure on AWS cloud platform.
