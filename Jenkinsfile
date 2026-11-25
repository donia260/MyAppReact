pipeline {
  agent any

  environment {
    PATH = "C:\\Program Files\\Docker\\Docker\\resources\\bin;${env.PATH}"
  }

  stages {
    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Build images') {
      steps { bat 'docker compose build' }
    }

    stage('Run') {
      steps {
        bat 'docker compose down'
        bat 'docker compose up -d'
      }
    }

    stage('Test') {
      steps {
        bat 'ping -n 15 127.0.0.1 > nul'
        bat 'curl -f http://localhost:9000/api/health'
        bat 'curl -f http://localhost:3000'
      }
    }
  }

  post {
    always  { bat 'docker compose down' }
    success { echo 'Build + Test OK' }
    failure { echo 'Build ou Test KO' }
  }
}