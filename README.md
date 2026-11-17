# HRMS Application Deployment on AWS EKS using DevOps

## Project Overview

This project demonstrates the deployment of a full-stack Human Resource
Management System (HRMS) application on AWS using DevOps tools and
technologies.

The HRMS application helps organizations manage employee information,
salary details, leave requests, and payment information.

The application is containerized using Docker and deployed on Amazon
Elastic Kubernetes Service (EKS). Jenkins automates the CI/CD pipeline
for building the application, creating Docker images, pushing images to
Amazon ECR, and deploying the application to Kubernetes.

## Application Features

-   Employee information management
-   Salary management
-   Leave management
-   Payment details management

## Technology Stack

  Category                 Technologies
  ------------------------ ----------------------------------------------
  Cloud Platform           AWS
  AWS Services             EC2, VPC, IAM, EKS, ECR, S3, ALB, CloudWatch
  Frontend                 Angular 17
  Backend                  Java 17, Spring Boot 3.5
  Database                 MySQL
  Containerization         Docker, Docker Compose
  Orchestration            Kubernetes, Amazon EKS
  CI/CD                    Jenkins
  Version Control          Git, GitHub
  Infrastructure as Code   Terraform
  Build Tools              Maven, npm
  Web Server               Nginx
  Operating System         Amazon Linux 2023

## Project Architecture

The following diagram represents the logical HRMS application deployment
workflow on AWS.

``` mermaid
flowchart TB
    DEV["Developer"]
    GH["GitHub Repository"]
    J["Jenkins Server - AWS EC2"]

    DEV --> GH
    GH --> J

    subgraph AWS["AWS Cloud"]
        subgraph CICD["CI/CD Pipeline"]
            J --> MAVEN["Maven Backend Build"]
            J --> NPM["npm Frontend Build"]
            MAVEN --> DOCKER["Docker Image Build"]
            NPM --> DOCKER
            DOCKER --> ECR["Amazon ECR"]
        end

        subgraph VPC["AWS VPC"]
            ALB["Application Load Balancer"]

            subgraph EKS["Amazon EKS Cluster"]
                ING["Kubernetes Ingress"]
                FS["Frontend Service"]
                BS["Backend Service"]
                FP["Angular Frontend Pods"]
                BP["Spring Boot Backend Pods"]
                MS["MySQL Service"]
                DB["MySQL Pod"]

                ING --> FS
                ING --> BS
                FS --> FP
                BS --> BP
                BP --> MS
                MS --> DB
            end

            ALB --> ING
        end

        TF["Terraform Infrastructure"]
        TF -.-> VPC
        ECR -.->|"Pull Docker Images"| FP
        ECR -.->|"Pull Docker Images"| BP
        J -->|"kubectl apply"| EKS
    end

    USER["Application User"]
    USER --> ALB
```

### Architecture Explanation

1.  Developer pushes application code to GitHub.
2.  Jenkins retrieves the source code from GitHub.
3.  Jenkins builds the backend using Maven.
4.  Jenkins builds the frontend using npm.
5.  Docker creates separate frontend and backend images.
6.  Docker images are pushed to Amazon ECR.
7.  Jenkins deploys Kubernetes configurations to Amazon EKS.
8.  Kubernetes pulls the Docker images from ECR.
9.  AWS Application Load Balancer receives external application traffic.
10. Kubernetes Ingress routes requests to frontend and backend services.
11. The Spring Boot backend communicates with MySQL.

## CI/CD Pipeline Using Jenkins

Jenkins automates the application build and deployment process.

### Jenkins Pipeline Stages

  Stage            Description
  ---------------- ------------------------------------
  Checkout         Pull source code from GitHub
  Backend Build    Build Spring Boot using Maven
  Frontend Build   Build Angular using npm
  Docker Build     Create frontend and backend images
  ECR Login        Authenticate with Amazon ECR
  Push Images      Push Docker images to ECR
  Deployment       Apply Kubernetes YAML files
  Rollout Status   Verify deployment rollout status

### Jenkins Pipeline Diagram

``` mermaid
flowchart TD
    A["GitHub Checkout"] --> B["Backend Build - Maven"]
    B --> C["Frontend Build - npm"]
    C --> D["Docker Image Build"]
    D --> E["Login to Amazon ECR"]
    E --> F["Push Images to ECR"]
    F --> G["kubectl apply"]
    G --> H["Kubernetes Deployment"]
    H --> I["Check Rollout Status"]
    I --> J["Application Deployed"]
```

## AWS Infrastructure

### Amazon EC2

-   Hosts the Jenkins server.
-   Provides the CI/CD execution environment.

### Amazon VPC

-   Provides isolated AWS networking.
-   Includes public and private subnets across Availability Zones.
-   Uses an Internet Gateway and NAT Gateway.
-   Configures route tables and security groups.

### Amazon EKS

-   Hosts the Kubernetes cluster.
-   Manages containerized applications.
-   Uses managed node groups.

### Amazon ECR

-   Stores frontend and backend Docker images.
-   Provides container images to Kubernetes.

### AWS Application Load Balancer

-   Receives external application traffic.
-   Routes requests through Kubernetes Ingress.

### AWS IAM

-   Manages permissions for AWS resources and services.

### Amazon CloudWatch

-   Provides AWS infrastructure and service monitoring capabilities.

## Kubernetes Deployment

The HRMS application is deployed on Amazon EKS using Kubernetes
resources.

### Kubernetes Components

  Component                   Purpose
  --------------------------- --------------------------------------
  Namespace                   Separates HRMS application resources
  Deployment                  Manages application pods
  Service                     Enables internal communication
  Secret                      Stores database credentials
  Ingress                     Routes incoming traffic
  Horizontal Pod Autoscaler   Automatically scales backend pods

### Application Traffic Routing

-   `/` routes requests to the Angular frontend service.
-   `/api` routes requests to the Spring Boot backend service.

### Horizontal Pod Autoscaler

Backend HPA configuration:

-   Minimum replicas: 2
-   Maximum replicas: 5
-   CPU utilization target: 70%

## Docker Implementation

### Backend

-   Base image: Eclipse Temurin Java 17.
-   Builds and packages the Spring Boot application.
-   Exposes application port 8080.

### Frontend

-   Uses Node.js 18 to build the Angular application.
-   Uses Nginx Alpine to serve frontend files.
-   Exposes application port 80.

## Infrastructure as Code - Terraform

Terraform configuration is maintained in a separate repository for AWS
infrastructure provisioning.

The infrastructure configuration includes:

-   AWS VPC.
-   Public and private subnets.
-   Internet Gateway and NAT Gateway.
-   Route tables and networking.
-   Amazon EKS cluster.
-   EKS managed node groups.

Terraform modules help organize infrastructure configuration and
simplify resource management.

**Related repository:** `hrms-eks-terraform`

## Project Implementation and Learning

Through this project, I gained practical experience in:

-   Building CI/CD pipelines using Jenkins.
-   Creating and managing Docker images.
-   Pushing container images to Amazon ECR.
-   Deploying applications on Amazon EKS.
-   Managing Kubernetes deployments and services.
-   Configuring AWS networking and security.
-   Configuring Kubernetes Ingress and Application Load Balancer.
-   Implementing Horizontal Pod Autoscaler.
-   Provisioning AWS infrastructure using Terraform.

## Future Enhancements

-   Integrate SonarQube for code quality analysis.
-   Configure Prometheus and Grafana for monitoring.
-   Introduce Helm charts for Kubernetes deployments.
-   Improve application security and deployment automation.

## Conclusion

This project demonstrates the implementation of DevOps practices for
deploying a full-stack HRMS application on AWS.

By combining Docker, Jenkins, Kubernetes, Amazon EKS, Amazon ECR, and
Terraform, the project provides practical experience in application
containerization, automated deployment, cloud infrastructure management,
and Kubernetes orchestration.

------------------------------------------------------------------------


passwords, access keys, tokens, or other secrets.

