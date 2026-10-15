
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'GitHub source code checked out'
            }
        }

        stage('Test') {
            steps {
                bat 'node --check app.js'
                echo 'Application test passed'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t soebus/devops-cicd-app:latest .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_TOKEN'
                    )
                ]) {
                    bat 'echo %DOCKER_TOKEN%| docker login --username %DOCKER_USER% --password-stdin'
                    bat 'docker push soebus/devops-cicd-app:latest'
                }
            }
        }
    }

    post {
        success {
            echo 'SUCCESS: Docker image pushed to Docker Hub!'
        }
        failure {
            echo 'FAILED: Check Console Output for details.'
        }
    }
}
