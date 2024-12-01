pipeline {
    agent any
    environment {
        DOCKER_HUB_REPO = 'mbeng44'
        IMAGE_TAG = 'latest'
        DOCKER_CREDENTIALS = '8461b105-3d7c-4d95-b34d-da4af62b3a16'
        KUBE_NAMESPACE = 'mbeng' 
    }
    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/mbeng44/worker.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    def appImage = docker.build("${mbeng44}/worker:${latest}")
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('', 8461b105-3d7c-4d95-b34d-da4af62b3a16) {
                        def appImage = docker.image("${mbeng44}/worker:${latest}")
                        appImage.push()
                    }
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh """
                        kubectl apply -f kubernetes/deployment.yaml --namespace=${mbeng}
                        kubectl apply -f kubernetes/service.yaml --namespace=${mbeng}
                    """
                }
            }
        }
    }
}

