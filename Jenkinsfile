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
        $image = "$env:IMAGE_NAME"
        docker build -t "$image:latest" .
        '''
      }
    }
    stage('Push to dockerhub') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'docker', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
          powershell '''
          $image = "$env:IMAGE_NAME"
          echo "$env:DOCKER_PASS" | docker login -u "$env:DOCKER_USER" --password-stdin
          docker push "$image:latest"
          docker logout
          '''
        }
      }
    }
  }
}
