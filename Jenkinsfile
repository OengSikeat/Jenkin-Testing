pipeline {
    agent any

    environment {
        IMAGE_NAME = 'test'
        CONTAINER_NAME = 'test'
        PORT = '8080'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            when {
                branch 'main'
            }
            steps {
                sh 'chmod +x gradlew'
                sh './gradlew clean build'
            }
        }

        stage('Test') {
            when {
                branch 'main'
            }
            steps {
                sh './gradlew test'
            }
        }

        stage('Docker Build') {
            when {
                branch 'main'
            }
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                sh '''
                docker stop $CONTAINER_NAME || true
                docker rm $CONTAINER_NAME || true
                docker run -d \
                  --name $CONTAINER_NAME \
                  -p $PORT:$PORT \
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
