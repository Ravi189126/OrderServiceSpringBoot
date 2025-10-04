pipeline {
    agent any

    environment {
        // Name of the Docker image you want to build
        DOCKER_IMAGE = "ravindra/orderservice"
        // Replace with your Docker Hub credentials ID in Jenkins
        DOCKER_CREDENTIALS = "docker-hub-credentials-id"
        GIT_REPO = "https://github.com/Ravi189126/OrderServiceSpringBoot.git"
        GIT_BRANCH = "main"
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo "Cloning Git repository..."
                git branch: "${GIT_BRANCH}", url: "${GIT_REPO}"
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                bat "docker build -t %DOCKER_IMAGE% ."
            }
        }

        stage('Push Docker Image') {
            steps {
                echo "Pushing Docker image to Docker Hub..."
                withCredentials([usernamePassword(
                    credentialsId: "${DOCKER_CREDENTIALS}",
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    bat """
                    docker login -u %DOCKER_USER% -p %DOCKER_PASS%
                    docker push %DOCKER_IMAGE%
                    """
                }
            }
        }
    }

    post {
        success {
            echo "Build and Docker push completed successfully!"
        }
        failure {
            echo "Build failed!"
        }
    }
}
