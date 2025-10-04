pipeline {
    agent any

    environment {
        // Docker image name
        DOCKER_IMAGE = "ravindra/demoapp"
        // Docker Hub credentials ID in Jenkins (optional)
        DOCKER_CREDENTIALS = "docker-hub-credentials-id"
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Pull code from GitHub
                git branch: 'main', url: 'https://github.com/Ravi189126/DemoWithJenking.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build docker image
                    sh "docker build -t ${DOCKER_IMAGE}:latest ."
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Login and push to Docker Hub
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS}") {
                        sh "docker push ${DOCKER_IMAGE}:latest"
                    }
                }
            }
        }
    }

    post {
        success {
            echo "Docker image built and pushed successfully!"
        }
        failure {
            echo "Build failed!"
        }
    }
}
