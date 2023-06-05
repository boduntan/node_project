pipeline {
  agent any
  
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub')
  }
  
  stages {
    stage('Build') {
      steps {
        // Checkout source code from the repository
        checkout scm
        
        // Build the Docker image
        script {
          docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            def dockerImage = docker.build('thecodegirl/thenodejs:latest', '--file Dockerfile .')
            dockerImage.push()
          }
        }
      }
    }
    
    stage('Login') {
      steps {
        withCredentials([usernamePassword(credentialsId: DOCKERHUB_CREDENTIALS, usernameVariable: 'DOCKERHUB_USERNAME', passwordVariable: 'DOCKERHUB_PASSWORD')]) {
          sh 'echo $DOCKERHUB_PASSWORD | docker login -u $DOCKERHUB_USERNAME --password-stdin'
        }
      }
    }
    
    stage('Push') {
      steps {
        sh 'docker push thecodegirl/thenodejs:latest'
      }
    }
    
    stage('Deploy') {
      steps {
        // Deploy the application using Docker Compose
        sh 'docker-compose up -d'
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
