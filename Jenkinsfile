pipeline {
    agent any

    stages {
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
        }

        stage('Code Analysis') {
            steps {
                echo 'Running code analysis...'
                bat 'mvn sonar:sonar'
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Running security scan...'
                bat 'mvn org.owasp:dependency-check-maven:check'
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to staging...'
                bat 'scp target/my-app.jar user@staging-server:/path/to/deploy'
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
                echo 'Deploying to production...'
                bat 'scp target/my-app.jar user@production-server:/path/to/deploy'
            }
        }
    }

    post {
        always {
            emailext (
                subject: "Pipeline Status: ${currentBuild.result ?: 'SUCCESS'}",
                body: "Stage: ${env.STAGE_NAME}\nStatus: ${currentBuild.result ?: 'SUCCESS'}\nLogs: ${env.BUILD_URL}console",
                to: 'your-email@example.com',
                attachLog: true
            )
        }
    }
}