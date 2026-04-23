pipeline {
  agent any

  environment {
    DOCKER_IMAGE = "sravya061994/devops-app"
    IMAGE_TAG = "${env.BRANCH_NAME}-${env.BUILD_NUMBER}"
  }

  stages {

    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build') {
      steps {
        sh 'mvn clean package'
      }
    }

    stage('Docker Build') {
      steps {
        sh 'docker build -t $DOCKER_IMAGE:$IMAGE_TAG .'
      }
    }

    stage('Docker Push') {
      when {
        branch 'main'
      }
      steps {
        withCredentials([usernamePassword(
          credentialsId: 'dockerhubcreds',
          usernameVariable: 'USER',
          passwordVariable: 'PASS'
        )]) {
          sh '''
          echo $PASS | docker login -u $USER --password-stdin
          docker push $DOCKER_IMAGE:$IMAGE_TAG
          '''
        }
      }
    }
  }
}
