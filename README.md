# Containerized-GameDay-App
## Project Overview

This project demonstrates building a containerized API management system for querying sports data. It leverages Amazon ECS (Fargate) for running containers, Amazon API Gateway for exposing REST endpoints, and an external Sports API for real-time sports data. 

## Features

* Exposes a REST API for querying real-time sports data
* Runs a containerized backend using Amazon ECS with Fargate
* Scalable and serverless architecture
* API management and routing using Amazon API Gateway

## Prerequisites

* Sports API Key: Sign up for a free account and subscription & obtain your API Key at serpapi.com
* AWS Account: Create an AWS Account & have basic understanding of ECS, API Gateway, Docker & Python
* AWS CLI Installed and Configured: Install & configure AWS CLI to programatically interact with AWS
* Serpapi Library: Install Serpapi library in local environment "pip install google-search-results"
* Docker CLI and Desktop Installed: To build & push container images

## Technologies

* Cloud Provider: AWS, Amazon ECS (Fargate), API Gateway, CloudWatch, IAM, API Gateway
* Programming Language: Python 3.x
* Containerization: Docker

## Project Structure

```
Containerized-GameDay-App/
├── app.py # Flask application for querying sports data
├── Dockerfile # Dockerfile to containerize the Flask app
├── requirements.txt # Python dependencies
├── .gitignore
└── README.md # Project documentation
```

# Setup Instructions
## Clone the Repository

`git clone https://github.com/ifeanyiro9/containerized-sports-api.git`

## Change directory into the clones repo

`cd containerized-sports-api`

## Create ECR Repo

`aws ecr create-repository --repository-name sports-api --region us-east-1`

## Authenticate and login to ECR

`aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com`

## Build image

`docker build --platform linux/amd64 -t sports-api .`

## Tag Image

`docker tag sports-api:latest <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/sports-api:sports-api-latest`

## Push Image to ECR repository

`docker push <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/sports-api:sports-api-latest`

# Set Up ECS Cluster with Fargate

1. Create an ECS Cluster:
* Go to the ECS Console → Clusters → Create Cluster
* Name your Cluster (sports-api-cluster)
* For Infrastructure, select Fargate, then create Cluster

2. Create a Task Definition:
* Go to Task Definitions → Create New Task Definition
* Name your task definition (sports-api-task)
* For Infrastructure, select Fargate
* Add the container:
    * Name your container (sports-api-container)
    * Image URI: <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/sports-api:sports-api-latest
    * Container Port: 8080
    * Protocol: TCP
    * Port Name: Leave Blank
    * App Protocol: HTTP
* Define Environment Variables:
    * Key: SPORTS_API_KEY
    * Value: <YOUR_SPORTSDATA.IO_API_KEY>
    * Create task definition

3. Run the Service with an ALB
* Go to Clusters → Select Cluster → Service → Create.
* Capacity provider: Fargate
* Select Deployment configuration family (sports-api-task)
* Name your service (sports-api-service)
* Desired tasks: 2
* Networking: Create new security group
* Networking Configuration
    * Type: All TCP
    * Source: Anywhere
* Load Balancing: Select Application Load Balancer (ALB).
* ALB Configuration:
* Create a new ALB:
* Name: sports-api-alb
* Target Group health check path: "/sports"
* Create service

4. Test the ALB:
* After deploying the ECS service, note the DNS name of the ALB (e.g., sports-api-alb-<AWS_ACCOUNT_ID>.us-east-1.elb.amazonaws.com)

* Confirm the API is accessible by visiting the ALB DNS name in your browser and adding /sports at end (e.g, http://sports-api-alb-<AWS_ACCOUNT_ID>.us-east-1.elb.amazonaws.com/sports)

# Configure API Gateway
1. Create a New REST API:
* Go to API Gateway Console → Create API → REST API
* Name the API (e.g., Sports API Gateway)

2. Set Up Integration:
* Create a resource /sports
* Create a GET method
* Choose HTTP Proxy as the integration type
* Enter the DNS name of the ALB that includes "/sports" (e.g. http://sports-api-alb-<AWS_ACCOUNT_ID>.us-east-1.elb.amazonaws.com/sports)

3. Deploy the API:
* Deploy the API to a stage (e.g., prod)
* Note the endpoint URL and test it to see if the application comes up.

## What We Learned
Setting up a scalable, containerized application with ECS Creating public APIs using API Gateway.

