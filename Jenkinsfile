pipeline {
    agent { label 'backend' }

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
                sh 'mvn -B clean compile -DskipTests'
            }
        }

        stage('Test') {
            steps {
                echo 'Running backend unit tests...'
                sh 'mvn -B test'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
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
