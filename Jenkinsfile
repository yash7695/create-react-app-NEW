pipeline {
  agent any

  tools {
    nodejs 'Node-18' // Must match your configured tool name in Jenkins
  }

  environment {
    CI = 'false' // Prevent CRA from treating warnings as errors
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
