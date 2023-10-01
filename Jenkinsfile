pipeline {
  agent any
  tools {
    maven 'maven_3.9.4'
  }

  stages {
    stage('Code Compilation') {
      steps {
        echo 'Code Compilation is In Progress!'
        sh 'mvn clean compile'
        echo 'Code Compilation is Completed Successfully!'
      }
    }

    stage('Code QA Execution') {
      steps {
        echo 'Junit Test case check in Progress!'
        sh 'mvn clean test'
        echo 'Junit Test case check Completed!'
      }
    }

    stage('Code Package') {
      steps {
        echo 'Creating War Artifact'
        sh 'mvn clean package'
        echo 'Creating War Artifact done'
      }
    }

    stage('Building & Tag Docker Image'){
      steps {
        echo 'Starting building docker image'
        sh 'docker build -t dharrsankube/flipkart-ms:dev-flipkart-ms-v1.${BUILD_NUMBER} .'
        sh 'docker build -t flipkart-ms:dev-flipkart-ms-v1.${BUILD_NUMBER} .'
        echo 'Completed building docker image'
      }
    }

    stage('Docker image scanning') {
      steps {
        echo 'Docker image scanning started'
        sh 'java --version'
      }
    }

    stage('Docker push to Docker Hub') {
      steps {
        script {
          withCredentials([string(credentialsId: 'dockerhubCred', variable: 'dockerhubCred')]) {
            sh 'docker login docker.io -u dharrsankube -p ${dockerhubCred}'
            echo 'Push Docker image to DockerHub: In Progress'
            sh 'docker push dharrsankube/flipkart-ms:dev-flipkart-ms-v1.${BUILD_NUMBER}'
            echo 'Push Docker image to DockerHub: Completed'
          }
        }
      }
    }



  }
}