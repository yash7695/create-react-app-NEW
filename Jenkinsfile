pipeline {
  agent any

  tools {
    nodejs 'Node-18'
  }

  environment {
    CI = 'false'
    AWS_DEFAULT_REGION = 'us-east-1' // change to your bucket's region
    BUCKET_NAME = 'yash-react-app'
  }

  stages {
    stage('Install Dependencies') {
      steps {
        sh 'npm ci'
      }
    }

    stage('Run Tests') {
      steps {
        sh 'npm test -- --watchAll=false'
      }
    }

    stage('Build React App') {
      steps {
        sh 'npm run build'
      }
    }

    stage('Deploy to S3') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'aws-creds', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
          sh '''
            aws s3 sync build/ s3://$BUCKET_NAME/ --delete
          '''
        }
      }
    }
  }

  post {
    success {
      echo '✅ Deployed to S3 successfully.'
    }
    failure {
      echo '❌ Deployment failed.'
    }
  }
}
