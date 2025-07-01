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
        script {
          // Groovy string interpolation happens here BEFORE it's passed to PowerShell
          def imageTag = "${env.IMAGE_NAME}:latest"
          powershell "docker build -t ${imageTag} ."
        }
      }
    }

    stage('Push to dockerhub') {
      steps {
        withCredentials([usernamePassword(
          credentialsId: 'docker',
          usernameVariable: 'DOCKER_USER',
          passwordVariable: 'DOCKER_PASS'
        )]) {
          script {
            def imageTag = "${env.IMAGE_NAME}:latest"
            powershell """
              echo $env:DOCKER_PASS | docker login -u $env:DOCKER_USER --password-stdin
              docker push ${imageTag}
              docker logout
            """
          }
        }
      }
    }
  }
}
