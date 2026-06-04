pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID     = credentials('aws-credentials').username
        AWS_SECRET_ACCESS_KEY = credentials('aws-credentials').password
        S3_BUCKET             = 'lms-aws-capstone'
        AWS_REGION            = 'us-east-1'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Deploy to S3') {
            steps {
                sh '''
                    aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID
                    aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY
                    aws configure set region $AWS_REGION
                    aws s3 sync site/ s3://$S3_BUCKET/ --acl public-read --delete
                '''
            }
        }
    }

    post {
        success {
            echo 'Website successfully deployed to S3!'
        }
        failure {
            echo 'Deployment failed. Please check the logs.'
        }
    }
}
