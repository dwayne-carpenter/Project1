pipeline {

  environment {
    registry = "dwaynecarpenter/project1"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }

  agent any

  tools {nodejs "node" }

  stages {

    stage('Clone Git') {
      steps {
        git 'https://github.com/dwayne-carpenter/Project1.git'
      }

    }

    stage('Build') {
       steps {
         sh 'npm install'
         sh 'cc -o cprog cprog.c'
       }

    }

    stage('Test') {

      steps {
        sh 'npm test'
        sh './cprog'
      }

    }

    stage('Build docker image') {

      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }

      }

    }

    stage('Deploy docker image to docker hub') {

      steps{
         script {
            docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }

        }

      }

    }

    stage('Clean up local docker image') {
       steps {
        sh "docker rmi $registry:$BUILD_NUMBER"
        sh "docker rmi -f $registry:latest"
        }
    }

  }

}

