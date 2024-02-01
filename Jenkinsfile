pipeline {
  agent {label 'clustermanager1'} 
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub')
  }
  stages {
    stage('Build') {
      steps {
        sh 'docker build -t meameise/meak8-webapp:latest .'
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push') {
      steps {
        sh 'docker push meameise/meak8-webapp:latest'
      }
    }
    stage('Deploy') {
            steps {
              script {
                   sh "kubectl apply -f nginx-deployment.yaml -f nginx-service.yaml"
                }
              }
            }
        }
    }
