HRMS Application Deployment on AWS EKS using DevOps

Project Overview

This project demonstrates the deployment of a full-stack Human Resource Management System (HRMS) application on AWS using DevOps tools and technologies.
The HRMS application helps organizations manage employee information, salary details, leave requests, and payment information.
The application is containerized using Docker and deployed on Amazon Elastic Kubernetes Service (EKS).
Jenkins is used to automate the CI/CD pipeline for application build and deployment.

Application Features

Employee information management

Salary management

Leave management

Payment details management

Architecture Explanation

Developer pushes application code to GitHub.

Jenkins retrieves the source code from GitHub.

Jenkins builds the backend using Maven.

Jenkins builds the frontend using npm.

Docker creates frontend and backend images.

Docker images are pushed to Amazon ECR.

Jenkins deploys Kubernetes configurations to Amazon EKS.

Kubernetes pulls Docker images from ECR.

AWS Application Load Balancer receives external traffic.

Kubernetes Ingress routes traffic to frontend and backend services.

Spring Boot backend communicates with MySQL
