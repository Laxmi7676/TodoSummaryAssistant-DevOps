pipeline {
    agent any

    environment {
        REGISTRY = "docker.io"
        IMAGE_NAME = "IMAGE_NAME = "laxmir22095/todo-summary-app"
        DOCKER_CREDS = credentials('dockerhub-creds')
    }

    stages {

        stage('Checkout') {
            steps {
                git url: 'https://github.com/Laxmi7676/TodoSummaryAssistant-DevOps'
            }
        }

        stage('Build Maven') {
            steps {
                sh '''
                cd Backend/todo-summary-assistant
                mvn clean package -DskipTests
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                cd Backend/todo-summary-assistant
                docker build -t $IMAGE_NAME:$BUILD_NUMBER .
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                sh '''
                echo $DOCKER_CREDS_PSW | docker login -u $DOCKER_CREDS_USR --password-stdin
                docker push $IMAGE_NAME:$BUILD_NUMBER
                '''
            }
        }
    }
}
~

