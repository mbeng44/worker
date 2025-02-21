
pipeline {
    agent any

    environment {
        AWS_ACCOUNT_ID = '205930645143' 
        AWS_REGION = 'us-east-1' 
        ECR_REPO_NAME = 'voting-app/worker1' 
        DOCKER_IMAGE_TAG = 'latest' 
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', 
                url: 'https://github.com/mbeng44/worker.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    docker.build("${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO_NAME}:${DOCKER_IMAGE_TAG}")
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Run tests (replace with your test command)
                    sh 'echo "Running tests for worker..."'
                }
            }
        }

        stage('Login to AWS ECR') {
            steps {
                script {
                    // Authenticate Docker to AWS ECR
                    sh "aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com"
                }
            }
        }

        stage('Push Docker Image to ECR') {
            steps {
                script {
                    // Push the Docker image to ECR
                    docker.withRegistry("https://${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com") {
                        docker.image("${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO_NAME}:${DOCKER_IMAGE_TAG}").push()
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Worker Docker image successfully built, tested, and pushed to ECR!'
        }
        failure {
            echo 'Pipeline failed. Check the logs for details.'
        }
    }
}


