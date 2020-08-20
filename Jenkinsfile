pipeline {
  environment {
    dockerRegistry = "nagasatishdocker/projects"
    dockerRegistryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent any
  tools {nodejs "nodejs" }
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/sanjeevkumarrao/node-hello-world'
      }
    }
    stage('Build') {
       steps {
         sh 'npm install'
       }
    }
    stage('test') {
      steps {
        sh 'npm test'
      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build dockerRegistry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Upload Image') {
      steps{
        script {
          docker.withRegistry( '', dockerRegistryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $dockerRegistry:$BUILD_NUMBER"
      }
    }
  }
}
