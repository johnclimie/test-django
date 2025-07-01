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
          # Disable BuildKit to avoid buildx issues
          $env:DOCKER_BUILDKIT=0

          # Tag image
          $image = "$env:IMAGE_NAME:latest"

          # Build docker image
          docker build -t $image .
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
            $image = "$env:IMAGE_NAME:latest"

            echo "$env:DOCKER_PASS" | docker login -u "$env:DOCKER_USER" --password-stdin
            docker push $image
            docker logout
          '''
        }
      }
    }
  }
}
