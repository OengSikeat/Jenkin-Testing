pipeline {
    agent any

    environment {
        IMAGE_NAME = 'test'
        CONTAINER_NAME = 'test'
        PORT = '8080' // Keep this 8080 (your app port)
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            // REMOVED: when { branch 'main' }
            steps {
                sh 'chmod +x gradlew'
                sh './gradlew clean build'
            }
        }

        stage('Test') {
            // REMOVED: when { branch 'main' }
            steps {
                sh './gradlew test'
            }
        }

        stage('Docker Build') {
            // REMOVED: when { branch 'main' }
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Deploy') {
            // REMOVED: when { branch 'main' }
            steps {
                sh '''
                docker stop $CONTAINER_NAME || true
                docker rm $CONTAINER_NAME || true

                # Maps Host Port 8080 -> Container Port 8080
                # (Safe now because Jenkins is on 9000)
                docker run -d \
                  --name $CONTAINER_NAME \
                  -p $PORT:8080 \
                  $IMAGE_NAME
                '''
            }
        }
    }

    post {
        success {
            echo 'Build and deploy successful'
        }
        failure {
            echo 'Pipeline failed'
        }
    }
}