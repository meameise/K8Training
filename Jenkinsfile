pipeline {
  agent {label 'dockerserver'} 
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub')
  }
  stages {
    stage('Build') {
      steps {
        sh 'docker build -t meameise/jenkins-nginx:devsecops-web .'
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push') {
      steps {
        sh 'docker push meameise/jenkins-nginx:devsecops-web'
      }
    }
    stage('Deploy') {
            steps {
              script {
                   sh "docker run -p 8081:80 -d meameise/jenkins-nginx:devsecops-web"
                }
              }
            }
        }
    }
