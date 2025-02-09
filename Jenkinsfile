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
                    sh 'docker run --name $CONTAINER_NAME -d $IMAGE_NAME' // הרצת הקונטיינר
                    sh 'sleep 10' // זמן להעלאת הקונטיינר
                    sh 'docker ps | grep $CONTAINER_NAME' // בדיקת יציבות
                    sh 'docker stop $CONTAINER_NAME && docker rm $CONTAINER_NAME' // ניקוי קונטיינר
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
