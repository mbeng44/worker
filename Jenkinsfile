pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = 'mbeng44'
        IMAGE_NAME = 'worker-app'
        IMAGE_TAG = 'latest'
        DOCKER_CREDENTIALS = '8461b105-3d7c-4d95-b34d-da4af62b3a16'
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    echo 'Cloning the worker repository...'
                    git branch: 'main', url: 'https://github.com/mbeng44/worker.git'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo 'Building Docker image...'
                    docker.build("${DOCKER_HUB_REPO}/${IMAGE_NAME}:${IMAGE_TAG}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    echo 'Pushing Docker image to DockerHub...'
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS}") {
                        docker.image("${DOCKER_HUB_REPO}/${IMAGE_NAME}:${IMAGE_TAG}").push()
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
        failure {
            echo 'Pipeline failed. Check logs for details.'
        }
    }


