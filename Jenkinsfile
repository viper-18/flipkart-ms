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

    stage('Docker Image Push to Amazon ECR') {
      steps {
        script {
          withDockerRegistry([credentialsId: 'ecr:ap-south-1:ecr-credentials', url: 'https://554138249436.dkr.ecr.ap-south-1.amazonaws.com']) {
            sh """
            docker tag flipkart-ms:dev-flipkart-v1.${BUILD_NUMBER} 554138249436.dkr.ecr.ap-south-1.amazonaws.com/flipkart-ms:${BUILD_NUMBER}
            docker push 554138249436.dkr.ecr.ap-south-1.amazonaws.com/flipkart-ms:dev-flipkart-v1.${BUILD_NUMBER}
            """
          }
        }
      }
    }

  }
}