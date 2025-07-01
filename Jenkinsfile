pipeline{
  agent any
  stages{
    stage('Checkout'){
      steps{
        git branch: 'main', url: 'https://github.com/Parth2k3/test-django'
      }
    }
    stage('Login to ECR'){
      steps{
        wtihAWS(region: 'us-east-2', credentials: 'aws-creds'){
          powershell '''
          $password = aws ecr get-login-password --region us-east-2
          docker login --username AWS --password $password 451947743265.dkr.ecr.us-east-2.amazonaws.com
          '''
        }
      }
    }
    stage('Build docker image'){
      steps{
        powershell '''
        docker build -t test:django
        docker tagtest:django 451947743265.dkr.ecr.us-east-2.amazonaws.com/test:django
        '''
      }
    }
    stage('Pushing image to ECR'){
      steps{
        powershell '''
        docker push 451947743265.dkr.ecr.us-east-2.amazonaws.com/test:django
        '''
      }
    }
  }
}
