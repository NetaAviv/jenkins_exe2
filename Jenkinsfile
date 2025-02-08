pipeline {
    agent any

    environment {
        IMAGE_NAME = "my-docker-app"
        CONTAINER_NAME = "my-docker-container"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME} ."
                }
            }
        }

        stage('Run Container for Testing') {
            steps {
                script {
                    // Stop and remove any existing container
                    sh "docker stop ${CONTAINER_NAME} || true"
                    sh "docker rm ${CONTAINER_NAME} || true"

                    // Run a new container
                    sh "docker run -d --name ${CONTAINER_NAME} ${IMAGE_NAME}"
                    
                    // Wait a bit for the container to initialize
                    sh "sleep 5"

                    // Check if the container is running
                    sh "docker ps | grep ${CONTAINER_NAME}"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo "Deploying container..."
                    // Add deployment steps if needed
                }
            }
        }
    }

    post {
        failure {
            echo 'Pipeline failed!'
            sh "docker logs ${CONTAINER_NAME} || true"
        }
        always {
            echo 'Cleaning up...'
            sh "docker stop ${CONTAINER_NAME} || true"
            sh "docker rm ${CONTAINER_NAME} || true"
        }
    }
}
