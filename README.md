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

