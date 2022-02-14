pipeline {
    agent any    
    environment{
        ecrurl = "057*********.dkr.ecr.us-east-1.amazonaws.com/my-nginx"
    }
    
    stages{
        stage('Checkout SCM') {
            steps {
            git branch: 'master',
            url: 'https://github.com/muzafferjoya/nginx_ecr.git'
            }
        }
        stage('Building image') {
           steps{
           script {
            dockerImage = docker.build ecrurl 
        }
      }
    }
    stage('Publishing To AWS ECR') {
      steps{
        script {
          sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 057*********.dkr.ecr.us-east-1.amazonaws.com'
          sh 'docker push 057*******.dkr.ecr.us-east-1.amazonaws.com/my-nginx:latest'
          //sh 'docker push 057***********.dkr.ecr.us-east-1.amazonaws.com/my-nginx:$BUILD_NUMBER'
            
        }
        }
      }
      stage('Stop Previous Container'){
        steps{
         sh 'docker ps -f name=mynginxcontainer -q | xargs --no-run-if-empty docker container stop'
         sh 'docker container ls -a -fname=mynginxcontainer -q | xargs -r docker container rm'
        }
      }
      stage('Docker Run'){
        steps{
          script{
            sh 'docker run -d -p 8080:80 --rm --name mynginxcontainer 057**********.dkr.ecr.us-east-1.amazonaws.com/my-nginx:latest'
          }
        }      
      }
      /*stage('Remove Unused docker image') {
      steps{
        //sh "docker rmi $ecrurl:$BUILD_NUMBER"
         sh "docker rmi $ecrurl:latest"

      }
    }*/
    }
}
