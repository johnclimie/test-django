pipeline{
  agent any
  environment{
    IMAGE_NAME = 'johnclimie/test-django'
  }
  stages{
    stage('Checkout'){
      steps{
        git branch: 'main', url: 'https://github.com/johnclimie/test-django'
      }
    }
    stage('Build docker image'){
      steps{
        powershell '''
        $tag = "$env:IMAGE_NAME:latest"
        docker build -t $tag .
        '''
      }
    }
   stage('Push to dockerhub'){
      steps{
        withCredentials([usernamePassword(credentialsId: 'docker', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]){
          powershell '''
          $tag = "$env:IMAGE_NAME:latest"
          echo "$env:DOCKER_PASS" | docker login -u "$env:DOCKER_USER" --password-stdin
          docker push $tag
          docker logout
          '''
        }
      }
    }
  }
}
