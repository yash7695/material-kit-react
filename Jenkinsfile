pipeline {
    agent any

    environment {
        AWS_REGION  = 'us-east-1'
        S3_BUCKET   = 'yash-react-app'             // 👈 Change to your bucket
    }

    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Project') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Upload to S3') {
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
            echo '✅ Successfully deployed to S3!'
        }
        failure {
            echo '❌ Pipeline failed. Check logs.'
        }
    }
}
