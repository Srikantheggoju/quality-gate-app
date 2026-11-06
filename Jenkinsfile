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

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh '''
                        sonar-scanner \
                        -Dsonar.projectKey=quality-gate-app \
                        -Dsonar.projectName=quality-gate-app \
                        -Dsonar.sources=. \
                        -Dsonar.python.version=3.11
                    '''
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
            echo '✅ SonarQube Analysis Successful'
            echo '✅ Docker Build Successful'
        }

        failure {
            echo '❌ Pipeline Failed'
        }

        always {
            cleanWs()
        }
    }
}
