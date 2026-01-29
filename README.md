DevOps Code Challenge – Full Stack Deployment

Overview
This repository contains a React frontend and an Express backend. The application is containerized, deployed to AWS, and made publicly accessible via an Application Load Balancer. Infrastructure is provisioned using Terraform, and CI/CD is handled through Jenkins.

Architecture Overview
Frontend: React (Dockerized)
Backend: Node.js / Express (Dockerized)
Container Registry: Amazon ECR
Compute: Amazon ECS
Load Balancing: Application Load Balancer (ALB)
Infrastructure as Code: Terraform
CI/CD: Jenkins (running in Docker on EC2)
Autoscaling: ECS Service Auto Scaling based on CPU utilization

Prerequisites
The following tools are required to deploy and operate this project:

AWS Account (Free Tier sufficient)

AWS CLI

Terraform

Docker

Node.js (for local development only)

Jenkins (configured as part of this project)

Git

Optional: Siege for load testing

Local Development (Optional)

Backend
cd backend
npm ci
npm start

The backend responds to requests on:
http://localhost:8080

Frontend
cd frontend
npm ci
npm start

The frontend runs on:
http://localhost:3000

If the frontend successfully connects to the backend, a SUCCESS message followed by a GUID is displayed.

Infrastructure Deployment (Terraform)

Configure AWS credentials
aws configure

Initialize Terraform
cd terraform
terraform init

Review and apply infrastructure
terraform plan
terraform apply

Terraform provisions:

VPC and networking

ECS cluster and services

Application Load Balancer

Auto Scaling configuration

IAM roles and policies

Jenkins Setup

Jenkins Host
Jenkins runs inside a Docker container on an EC2 instance. The Docker socket is mounted to allow Jenkins to build Docker images and push them to ECR.

Jenkins UI is accessible at:
http://50.16.243.13:8080

Jenkins Pipeline
The Jenkins pipeline performs the following steps:

Checkout source code

Build frontend and backend Docker images

Authenticate to Amazon ECR

Push images to ECR

Force new ECS service deployments

The pipeline configuration is defined in the Jenkinsfile at the root of the repository.

Deployment Flow (CI/CD)

Code is pushed to GitHub

Jenkins pipeline is triggered

Docker images are built

Images are pushed to Amazon ECR

ECS services are updated

Application Load Balancer routes traffic to updated containers

Public URLs

Frontend Application:
http://devops-challenge-alb-958422082.us-east-1.elb.amazonaws.com

Jenkins UI:
http://50.16.243.13:8080

Load Testing and Auto Scaling

Tool Used
Siege

Test Command
siege -c 250 -t 2M http://devops-challenge-alb-958422082.us-east-1.elb.amazonaws.com

Observed Results

ECS task count increased automatically during load

CPU utilization triggered ECS service auto scaling

Application remained available with approximately 99 percent uptime during testing

Cleanup

To destroy all AWS resources created by Terraform:
cd terraform
terraform destroy

Evaluation Notes
This project demonstrates reproducible infrastructure, automated CI/CD pipelines, containerized workloads, ECS auto scaling, and clear documentation to allow easy replication of the environment.

Submission

Private GitHub repository containing all code
https://github.com/waterparkpolo/devops-code-challenge1
Updated README with setup, deployment, and scaling details

Jenkins URL and credentials
http://50.16.243.13:8080
username: admin
password: adminpass
Public frontend URL
http://devops-challenge-alb-958422082.us-east-1.elb.amazonaws.com