pipeline {
    agent any

    tools {
        maven 'Maven 3.9.9' // Use the Maven version configured in Jenkins
    }

    environment {
        GIT_REPO = 'https://github.com/your-username/your-repo.git'
        EMAIL_RECIPIENT = 'your-email@example.com'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: env.GIT_REPO
            }
        }

        stage('Build') {
            steps {
                echo 'Building the code...'
                bat 'mvn clean package'
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo 'Running unit and integration tests...'
                bat 'mvn test'
                bat 'mvn verify'
            }
            post {
                always {
                    archiveArtifacts artifacts: '**/target/surefire-reports/*.xml', allowEmptyArchive: true
                }
                success {
                    emailext subject: "Jenkins: Tests Passed ✅",
                            body: "Unit & Integration Tests completed successfully.",
                            to: env.EMAIL_RECIPIENT
                }
                failure {
                    emailext subject: "Jenkins: Tests Failed ❌",
                            body: "Tests failed! Check Jenkins for details.",
                            to: env.EMAIL_RECIPIENT
                }
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Running security scan...'
                bat 'mvn org.owasp:dependency-check-maven:check'
            }
            post {
                success {
                    emailext subject: "Jenkins: Security Scan Passed ✅",
                            body: "Security Scan completed successfully.",
                            to: env.EMAIL_RECIPIENT
                }
                failure {
                    emailext subject: "Jenkins: Security Scan Failed ❌",
                            body: "Security Scan found vulnerabilities! Check Jenkins for details.",
                            to: env.EMAIL_RECIPIENT
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to Staging Server...'
                bat 'scp target/my-app.jar user@staging-server:/deployments/'
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging...'
                bat 'curl -X POST http://staging-server:8080/test-endpoint'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Deploying to Production Server...'
                bat 'scp target/my-app.jar user@production-server:/deployments/'
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed!'
        }
    }
}
