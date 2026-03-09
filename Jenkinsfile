pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/johneliud/mr-jenk.git'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Cloning repository...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building application...'
                // Placeholder for Maven/Angular build commands
            }
        }

        stage('Test') {
            steps {
                echo 'Running automated tests...'
                // Placeholder for JUnit and Jasmine/Karma tests
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                // Placeholder for deployment scripts
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution complete.'
        }
        success {
            echo 'Build and test successful.'
        }
        failure {
            echo 'Build or test failed. Please check logs.'
        }
    }
}
