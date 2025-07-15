pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID     = credentials('AWS_ACCESS_KEY_ID')   // Jenkins credentials ID
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
        AWS_REGION            = 'ap-south-1'                        // your AWS region
        S3_BUCKET             = 'your-s3-bucket-name'               // replace with your bucket
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
            echo '✅ Deployment to S3 completed successfully!'
        }
        failure {
            echo '❌ Deployment failed!'
        }
    }
}
