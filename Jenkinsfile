pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = 'mbeng44'
        IMAGE_NAME = 'worker'
        IMAGE_TAG = 'latest'
        DOCKER_CREDENTIALS = '8461b105-3d7c-4d95-b34d-da4af62b3a16'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/mbeng44/worker.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${mbeng44}/${worker}:${latest}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 8461b105-3d7c-4d95-b34d-da4af62b3a16) {
                        docker.image("${mbeng44}/${worker}:${latest}").push()
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}

    }
}
