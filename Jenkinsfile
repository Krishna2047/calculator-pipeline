pipeline {
    agent any

    environment {
        IMAGE_NAME = "krishna20047/calculator-app"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Docker Build') {
            steps {
                bat 'docker build -t %IMAGE_NAME%:%BUILD_NUMBER% .'
            }
        }

        stage('Test Inside Container') {
            steps {
                bat 'docker run %IMAGE_NAME%:%BUILD_NUMBER%'
            }
        }

        stage('Docker Tag') {
            steps {
                bat 'docker tag %IMAGE_NAME%:%BUILD_NUMBER% %IMAGE_NAME%:latest'
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    bat 'echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin'
                    bat 'docker push %IMAGE_NAME%:%BUILD_NUMBER%'
                    bat 'docker push %IMAGE_NAME%:latest'
                }
            }
        }
    }

    post {
        failure {
            echo "Pipeline failed!"
        }
        success {
            echo "Pipeline succeeded!"
        }
    }
}