pipeline {
    agent any

    environment {
        IMAGE_NAME = 'my-app'
        CONTAINER_NAME = 'my-app-container'
        REGISTRY = 'my-docker-registry.com'
    }

    triggers {
        pollSCM('* * * * *') // מריץ את הפייפליין בכל push ל-repo
    }

    stages {
        stage('Build') {
            steps {
                script {
                    sh 'docker build -t $IMAGE_NAME .' // בניית התמונה
                }
            }
        }

    stage('Test') {
        steps {
            script {
                sh '''
                    docker stop my-app-container || true
                    docker rm my-app-container || true
                    docker run --name my-app-container -d my-app
                    sleep 10
                    if ! docker ps | grep -q my-app-container; then
                        echo "Container failed to start"
                        docker logs my-app-container
                        exit 1
                    fi
                '''
            }
        }
    }

        stage('Deploy') {
            steps {
                script {
                    sh 'docker tag $IMAGE_NAME $REGISTRY/$IMAGE_NAME:latest'
                    sh 'docker push $REGISTRY/$IMAGE_NAME:latest' // פריסה לרג'יסטרי
                }
            }
        }
    }
}
