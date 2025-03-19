pipeline {
    agent any

    environment {
      
        GIT_REPO_URL = 'https://github.com/saamy45/TASK-6.2C'
        GIT_BRANCH = 'main'
    }

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    checkout([$class: 'GitSCM',
                        branches: [[name: "*/${GIT_BRANCH}"]],
                        userRemoteConfigs: [[
                            url: "${GIT_REPO_URL}",
                            credentialsId: "${GIT_CREDENTIALS_ID}"
                        ]]
                    ])
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    echo "Building the project..."
                    bat 'mvn clean package' // Use 'sh' instead of 'bat' for Linux/Mac
                }
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                script {
                    echo "Running tests..."
                    bat 'mvn test' // Runs unit tests
                }
            }
        }

        stage('Security Scan') {
            steps {
                script {
                    echo "Running security scans..."
                    bat 'mvn verify' // Replace with security scan tool if needed
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                script {
                    echo "Deploying to Staging..."
                    bat 'mvn deploy' // Adjust according to your deployment setup
                }
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                script {
                    echo "Running integration tests on Staging..."
                    bat 'mvn verify' // Replace with actual integration test script
                }
            }
        }

        stage('Deploy to Production') {
            steps {
                script {
                    echo "Deploying to Production..."
                    bat 'mvn deploy -P production' // Replace with actual deployment command
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline execution completed successfully!"
        }
        failure {
            echo "Pipeline execution failed!"
        }
    }
}
