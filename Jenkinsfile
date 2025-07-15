pipeline {
    agent any

    tools {
        nodejs 'Node18'  // Replace with the exact name you configured in Jenkins
    }

    environment {
        AWS_REGION  = 'us-east-1'
        S3_BUCKET   = 'your-s3-bucket-name'          // 🔁 Change this
    }

    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build React App') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Deploy to S3') {
            steps {
                withAWS(region: "${AWS_REGION}", credentials: 'aws-credentials-id') {
                    sh '''
                        aws s3 sync build/ s3://$S3_BUCKET/ --delete
                    '''
                }
            }
        }
    }

    post {
        success {
            echo '✅ S3 Deployment Successful!'
        }
        failure {
            echo '❌ Pipeline failed.'
        }
    }
}
