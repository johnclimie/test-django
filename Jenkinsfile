pipeline {
  agent any

  environment {
    IMAGE_NAME = 'johnclimie/test-django'
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/johnclimie/test-django'
      }
    }

    stage('Build docker image') {
      steps {
        powershell '''
          # Disable BuildKit to avoid buildx errors
          $env:DOCKER_BUILDKIT=0

          # Run docker build with resolved environment variable
          docker build -t "$env:IMAGE_NAME:latest" .
        '''
      }
    }

    stage('Push to dockerhub') {
      steps {
        withCredentials([usernamePassword(
          credentialsId: 'docker',
          usernameVariable: 'DOCKER_USER',
          passwordVariable: 'DOCKER_PASS'
        )]) {
          powershell '''
            docker login -u "$env:DOCKER_USER" --password-stdin <<< "$env:DOCKER_PASS"
            docker push "$env:IMAGE_NAME:latest"
            docker logout
          '''
        }
      }
    }
  }
}
