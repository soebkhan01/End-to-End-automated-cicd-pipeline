
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Source code is ready in the workspace'
            }
        }

        stage('Test') {
            steps {
                bat 'node --check app.js'
                echo 'Application syntax test passed!'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t devops-cicd-app .'
            }
        }

        stage('Deploy Locally') {
            steps {
                bat '''
                    docker rm -f devops-cicd-container || exit /b 0
                    docker run -d --name devops-cicd-container -p 3000:3000 devops-cicd-app
                '''
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the Console Output.'
        }
    }
}
