pipeline {
    agent any

    environment {
        IMAGE_NAME = "srikantheggoju/quality-gate-app"
    }

    tools {
        sonarQubeScanner 'sonar-scanner'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh 'sonar-scanner'
                }
            }
        }

        stage('Docker Build') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:latest ."
            }
        }

        stage('Verify Image') {
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
