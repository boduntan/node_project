pipeline {
  agent any
  
  stages {
    stage('Build') {
      steps {
        // Checkout source code from the repository
        checkout scm

        // Build the Docker image
        script {
            withCredentials([string(credentialsId: 'dckr_pat_z6Y0LmjH555zKWUX-tDUQbON0Kc', variable: 'DOCKER_ACCESS_TOKEN')]) {
            docker.withRegistry('', 'docker') {
              def dockerImage = docker.build('thecodegirl/thenodejs:latest', '--file Dockerfile .')
              dockerImage.push()
            }
        }
      }
    }
    stage('Deploy') {
      steps {
        // Deploy the application using Docker Compose
        script {
          sh 'docker-compose up -d'
        }
      }
    }
    }
  post {
    always {
      // Clean up Docker resources after deployment
      sh 'docker-compose down'
    }
  }
 }
}
