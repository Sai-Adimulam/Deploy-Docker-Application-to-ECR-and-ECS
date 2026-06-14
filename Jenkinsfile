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
