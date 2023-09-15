pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                sh 'mvn test'
                sh 'mvn verify'
            }
        }

        stage('Code Analysis') {
            steps {
                sh 'sonar-scanner'
            }
        }

        stage('Security Scan') {
            steps {
                sh 'zap-cli --quick-scan https://myapp.com'
            }
        }

        stage('Deploy to Staging') {
            steps {
                sh 'aws s3 cp myapp-1.0.0.jar s3://my-app-bucket/staging/'
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                sh 'mvn verify -Pstaging'
            }
        }

        stage('Deploy to Production') {
            steps {
                sh 'aws s3 cp myapp-1.0.0.jar s3://your-s3-bucket/production/'
            }
        }
    }

    post {
        success {
            emailext subject: 'Pipeline Status: SUCCESS',
                body: 'Pipeline executed successfully.',
                attachmentsPattern: '**/*'
        }
        failure {
            emailext subject: 'Pipeline Status: FAILURE',
                body: 'Pipeline failed. Please check the logs.',
                attachmentsPattern: '**/*'
        }
    }
}
