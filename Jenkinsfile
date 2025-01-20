pipeline {
    agent any
    environment {
        IMAGE_NAME = "pjbaur/backend"
        IMAGE_TAG = "${env.BUILD_NUMBER}"
    }
    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/pjbaur/backend-repo.git'
            }
        }
        stage('Build') {
            steps {
                sh './mvnw clean package -DskipTests'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
            }
        }
        stage('Push Docker Image') {
            steps {
                withDockerRegistry([credentialsId: 'docker-hub-credentials']) {
                    sh 'docker push $IMAGE_NAME:$IMAGE_TAG'
                }
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker rm -f backend || true'
                sh 'docker run -d --name backend -p 8080:8080 $IMAGE_NAME:$IMAGE_TAG'
            }
        }
    }
}
