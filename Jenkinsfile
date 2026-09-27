pipeline {
    agent any

    environment {
        IMAGE_NAME = "ci-cd-demo"
        CONTAINER_NAME = "ci-cd-demo-container"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Test') {
            steps {
                sh 'docker run --rm $IMAGE_NAME nginx -t'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker stop $CONTAINER_NAME || true
                    docker rm $CONTAINER_NAME || true
                    docker run -d --name $CONTAINER_NAME -p 8081:80 $IMAGE_NAME
                '''
            }
        }
    }

    post {
        success {
            echo 'Развёртывание выполнено успешно!'
        }
        failure {
            echo 'Ошибка в процессе выполнения pipeline.'
        }
    }
}
