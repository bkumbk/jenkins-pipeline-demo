pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo "Running on branch: ${env.BRANCH_NAME}"
                echo 'Code checked out successfully!'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the app...'
            }
        }

        stage('Build Docker Image') {
            when {
                branch 'main'
            }
            steps {
                echo 'On main branch — Building Docker image...'
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                echo 'On main branch — Deploying app...'
            }
        }

    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
