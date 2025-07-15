pipeline {
    agent any

    tools {
        nodejs 'node18'  // ✅ Must match exactly with Jenkins Global Tool Configuration
    }

    environment {
        AWS_REGION = 'us-east-1'                       // ✅ Update your region
        S3_BUCKET = 'your-s3-bucket-name'              // ✅ Replace with your S3 bucket name
        BUILD_DIR = 'build'                            // React app build folder
        NODE_OPTIONS = '--openssl-legacy-provider'     // Fix for crypto error on Node 17+
    }

    stages {
        stage('Install Dependencies') {
            steps {
                echo '📦 Installing npm packages...'
                sh 'npm install'
            }
        }

        stage('Build React App') {
            steps {
                echo '🛠️ Building React App...'
                sh 'npm run build'
            }
        }

        stage('Deploy to S3') {
            steps {
                echo '🚀 Uploading build folder to S3...'
                sh '''
                    aws s3 cp $BUILD_DIR s3://$S3_BUCKET/ --recursive --region $AWS_REGION
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Deployed successfully to S3!'
        }
        failure {
            echo '❌ Pipeline failed. Please check the logs.'
        }
    }
}
