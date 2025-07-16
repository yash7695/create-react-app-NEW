pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'ap-south-1'
        S3_BUCKET = 'your-s3-bucket-name' // 🔁 Change to your actual bucket
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Check Environment') {
         steps {
                 sh 'echo $PATH'
                 sh 'which node || echo "Node not found"'
                 sh 'which npm || echo "NPM not found"'
                 sh 'node -v || echo "No node version"'
                 sh 'npm -v || echo "No npm version"'
            }
        }


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
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-credentials-id' // 🔁 Replace with your Jenkins credentials ID
                ]]) {
                    sh 'aws s3 sync build/ s3://$S3_BUCKET --delete'
                }
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful!'
        }
        failure {
            echo '❌ Deployment failed. Please check the logs.'
        }
    }
}
