pipeline {
    agent any

    environment {
        IMAGE_NAME = "yourdockerhubusername/calculator-app"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Test') {
            steps {
                sh 'pytest'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t $IMAGE_NAME:${BUILD_NUMBER} .'
            }
        }

        stage('Docker Tag') {
            steps {
                sh 'docker tag $IMAGE_NAME:${BUILD_NUMBER} $IMAGE_NAME:latest'
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'krishna20047',
                    passwordVariable: 'IshuKrishna@6'
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker push $IMAGE_NAME:${BUILD_NUMBER}'
                    sh 'docker push $IMAGE_NAME:latest'
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