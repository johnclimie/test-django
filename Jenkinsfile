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
          $env:DOCKER_BUILDKIT=0
          docker build -t ${env.IMAGE_NAME}:latest .
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
            echo ${env.DOCKER_PASS} | docker login -u ${env.DOCKER_USER} --password-stdin
            docker push ${env.IMAGE_NAME}:latest
            docker logout
          '''
        }
      }
    }
  }
}
