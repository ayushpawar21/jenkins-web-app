pipeline {
  agent any
  stages {
    stage("verify tooling") {
      steps {
        sh '''
          docker version
          docker info
          docker compose version 
          curl --version
          jq --version
        '''
      }
    }
    stage('Prune Docker data') {
      steps {
        sh 'docker compose down'
      }
    }
    stage('Start container') {
      steps {
        sh 'docker compose up -d'
        sh 'docker compose ps'
      }
    }
    stage('Run tests against the container') {
      steps {
        sh 'docker ps'
      }
    }
  }
  post {
    always {
      sh 'docker compose ps'
    }
   }
}