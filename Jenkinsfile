pipeline {
    agent any

    tools {
        nodejs 'node16'  // ✅ Set this in Jenkins Global Tool Configuration
    }

    environment {
        AWS_REGION = 'us-east-1'                      // 🔧 Your AWS region
        S3_BUCKET = 'your-s3-bucket-name'             // 🔧 Your S3 bucket name
        BUILD_DIR = 'build'                           // 🔧 React build folder
        NODE_OPTIONS = '--openssl-legacy-provider'    // 🔧 Crypto fix for Node 17+
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
                sh '''
                    echo "📦 Uploading $BUILD_DIR to S3..."
                    aws s3 cp $BUILD_DIR s3://$S3_BUCKET/ --recursive --region $AWS_REGION
                '''
            }
        }
    }

    post {
        success {
            echo '✅ React app deployed to S3 successfully!'
        }
        failure {
            echo '❌ Pipeline failed. Check the logs above.'
        }
    }
}
