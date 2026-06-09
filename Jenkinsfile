pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Deploy to S3') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'aws-credentials', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    sh '''
                        aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID
                        aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY
                        aws configure set region us-east-1
                        aws s3 sync site/ s3://lms-aws-capstone/ --acl public-read --delete
                    '''
                }
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
