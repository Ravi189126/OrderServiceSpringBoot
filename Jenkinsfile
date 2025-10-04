pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "ravindra/orderservice"
        DOCKER_CREDENTIALS = "dockerhub"  // This matches the ID you just created
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
                    docker logout
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
