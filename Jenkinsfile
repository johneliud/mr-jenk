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

        stage('Build & Test Services') {
            parallel {
                stage('user-service') {
                    steps {
                        build job: 'user-service', wait: true
                    }
                }
                stage('product-service') {
                    steps {
                        build job: 'product-service', wait: true
                    }
                }
                stage('media-service') {
                    steps {
                        build job: 'media-service', wait: true
                    }
                }
                stage('api-gateway') {
                    steps {
                        build job: 'api-gateway', wait: true
                    }
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
            echo 'All services built and tested successfully.'
        }
        failure {
            echo 'One or more services failed. Check individual job logs.'
        }
    }
}
