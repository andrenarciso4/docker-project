pipeline {
  environment {
    registry = "andrenarciso4/docker-jenkins"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent any
  //tools {nodejs 'node' }
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/andrenarciso4/docker-project.git'
      }
    }
    /*stage('Build') {
       steps {
         sh 'npm install'
         sh 'npm run bowerInstall'
       }
    }
    stage('Test') {
      steps {
        sh 'npm test'
      }
    }*/
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Deploy Image') {
      steps{
         script {
            docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
  }
}
