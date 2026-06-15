Deploy Docker Application to AWS ECR & ECS Using Jenkins
Project Overview

This project demonstrates a complete CI/CD pipeline using Jenkins to build a Docker image, push it to Amazon Elastic Container Registry (ECR), and deploy the application to Amazon Elastic Container Service (ECS).

Architecture

GitHub → Jenkins → Docker Build → Amazon ECR → Amazon ECS → Application

Technologies Used
Jenkins
Docker
AWS ECR
AWS ECS
GitHub
Python Flask
Ubuntu Linux
Prerequisites
AWS Account
Jenkins Server
Docker Installed
AWS CLI Configured
GitHub Repository
IAM User with ECR and ECS Permissions
Project Structure
.
├── app.py
├── requirements.txt
├── Dockerfile
├── Jenkinsfile
└── README.md
Step 1: Clone the Repository
git clone https://github.com/your-github-username/Deploy-Docker-Application-to-ECR-and-ECS.git
cd Deploy-Docker-Application-to-ECR-and-ECS
Step 2: Create Dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY . .

RUN pip install -r requirements.txt

EXPOSE 5000

CMD ["python","app.py"]
Step 3: Create Amazon ECR Repository
Login to AWS Console
Navigate to ECR
Create Repository
Copy Repository URI

Example:

908708651361.dkr.ecr.us-east-1.amazonaws.com/flask-app
Step 4: Configure Jenkins
Install Required Plugins
Docker Pipeline
Pipeline
Git
AWS Credentials
Configure AWS Credentials

Manage Jenkins → Credentials → Add Credentials

Add:

AWS Access Key ID
AWS Secret Access Key
Step 5: Jenkins Pipeline
pipeline{
    agent any

    stages{
        stage('Github'){
            steps{
             git branch : 'main',
             url: 'https://github.com/Sai-Adimulam/Deploy-Docker-Application-to-ECR-and-ECS.git'
            }
        }
        stage('Build Docker-Image'){
            steps{
                sh 'docker build -t app-1:v1 .'
            }
        }
        stage('Authenticate with ECR'){
             steps{
                sh 'aws ecr get-login-password --region ap-south-2 |docker login --username AWS --password-stdin 908708651361.dkr.ecr.ap-south-2.amazonaws.com'
             }
        }
        stage('Tag Image to Ecr'){
            steps{
                sh 'docker tag app-1:v1 908708651361.dkr.ecr.ap-south-2.amazonaws.com/app-1:v1'
            }
        }
        stage('Push Image to Ecr'){
            steps{
                sh 'docker push 908708651361.dkr.ecr.ap-south-2.amazonaws.com/app-1:v1'
            }
        }
        
    }
}
Step 6: Create ECS Cluster
Open ECS Console
Click Create Cluster
Select ECS Fargate
Enter Cluster Name
Create Cluster
Step 7: Create Task Definition
ECS → Task Definitions
Create Task Definition
Launch Type: Fargate

Container Image:

ACCOUNT_ID.dkr.ecr.REGION.amazonaws.com/flask-app:latest

Port Mapping:

5000
Step 8: Create ECS Service
Open ECS Cluster
Create Service
Select Task Definition
