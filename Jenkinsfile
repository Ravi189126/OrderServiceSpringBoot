pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "ravindra/orderservice"
        DOCKER_USER = "ravi260"          // Docker Hub username
        DOCKER_PASS = "Ravindra@2"       // Docker Hub password
        GIT_REPO = "https://github.com/Ravi189126/OrderServiceSpringBoot.git"
        GIT_BRANCH = "main"
    }

    stages {
        stage('Checkout Code ') {
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
                bat """
                    docker login -u %DOCKER_USER% -p %DOCKER_PASS%
                    docker push %DOCKER_IMAGE%
                    docker logout
                """
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
