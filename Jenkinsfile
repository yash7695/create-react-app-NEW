pipeline {
  agent any

  tools {
    nodejs 'Node-18' // Make sure this matches your Jenkins NodeJS tool name
  }

  environment {
    CI = 'false' // Avoid Create React App exiting on warnings
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

    stage('Archive Build Artifacts') {
      steps {
        archiveArtifacts artifacts: 'build/**', fingerprint: true
      }
    }
  }

  post {
    success {
      echo '✅ Build succeeded.'
    }
    failure {
      echo '❌ Build failed.'
    }
    always {
      cleanWs()
    }
  }
}
