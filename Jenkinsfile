pipeline {
    agent any

    environment {
        IMAGE_NAME = "srikantheggoju/quality-gate-app"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('SonarQube Scan') {
            steps {
                echo 'Running SonarQube Analysis'
            }
        }

        stage('Quality Gate') {
            steps {
                echo 'Verifying Quality Gate'
            }
        }

        stage('Docker Build') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:latest ."
            }
        }

        stage('Docker Image Verification') {
            steps {
                sh "docker images | grep quality-gate-app"
            }
        }

    }

    post {
        success {
            echo 'Pipeline Completed Successfully'
        }

        failure {
            echo 'Pipeline Failed'
        }
    }
}
