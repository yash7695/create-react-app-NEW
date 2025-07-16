pipeline {
  agent any

  tools {
    nodejs 'Node18' // Use the NodeJS version installed in Jenkins (must match the name)
  }

  environment {
    CI = 'false' // Prevent Create React App from exiting on warnings
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

    stage('Archive Production Build') {
      steps {
        archiveArtifacts artifacts: 'build/**', fingerprint: true
      }
    }
  }

  post {
    success {
      echo "✅ React app built successfully."
    }
    failure {
      echo "❌ Build failed. Please check the logs."
    }
    always {
      cleanWs()
    }
  }
}
