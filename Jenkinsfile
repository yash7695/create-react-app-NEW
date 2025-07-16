pipeline {
  agent any

  tools {
    nodejs 'Node-18'
  }

  environment {
    CI = 'false'
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

    stage('Serve Build (Optional)') {
      steps {
        sh '''
          npm install -g serve
          serve -l 3000 build &
          sleep 10
          curl -I http://localhost:3000 || true
        '''
      }
    }

    stage('Archive Artifacts') {
      steps {
        archiveArtifacts artifacts: 'build/**', fingerprint: true
      }
    }
  }

  post {
    always {
      cleanWs()
    }
  }
}
