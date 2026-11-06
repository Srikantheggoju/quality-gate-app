pipeline {
    agent any

    environment {
        IMAGE_NAME = "srikantheggoju/quality-gate-app"
    }

    tools {
        sonarRunner 'sonar-scanner'
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

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
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
            echo '✅ SonarQube Scan Passed'
            echo '✅ Quality Gate Passed'
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
