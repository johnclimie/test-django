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
        # Disable BuildKit to prevent routing to buildx
        $env:DOCKER_BUILDKIT=0

        # Set image tag
        $image = "$env:IMAGE_NAME:latest"

        # Run docker build with correct context
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
        )]
