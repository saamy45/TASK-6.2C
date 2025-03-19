pipeline {
    agent any

    stages {
        // Stage 1: Build
        stage('Build') {
            steps {
                echo 'Building the code...'
                sh 'mvn clean package' // Example using Maven
            }
        }

        // Stage 2: Unit and Integration Tests
        stage('Unit and Integration Tests') {
            steps {
                echo 'Running unit and integration tests...'
                sh 'mvn test' // Example using Maven for unit tests
                sh 'mvn verify' // Example using Maven for integration tests
            }
        }

        // Stage 3: Code Analysis
        stage('Code Analysis') {
            steps {
                echo 'Running code analysis...'
                sh 'mvn sonar:sonar' // Example using SonarQube
            }
        }

        // Stage 4: Security Scan
        stage('Security Scan') {
            steps {
                echo 'Running security scan...'
                sh 'mvn org.owasp:dependency-check-maven:check' // Example using OWASP Dependency-Check
            }
        }

        // Stage 5: Deploy to Staging
        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to staging...'
                sh 'scp target/my-app.jar user@staging-server:/path/to/deploy' // Example using SCP
            }
        }

        // Stage 6: Integration Tests on Staging
        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging...'
                sh 'curl -X POST http://staging-server:8080/test-endpoint' // Example using curl
            }
        }

        // Stage 7: Deploy to Production
        stage('Deploy to Production') {
            steps {
                echo 'Deploying to production...'
                sh 'scp target/my-app.jar user@production-server:/path/to/deploy' // Example using SCP
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