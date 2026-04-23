pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "sravya061994/devops-app"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build (Maven)') {
            steps {
                sh "mvn clean package"
            }
        }

        stage('Prepare Tag') {
            steps {
                script {
                    def safeBranch = env.BRANCH_NAME.replaceAll('/', '-')
                    env.IMAGE_TAG = "${safeBranch}-${BUILD_NUMBER}"
                }
            }
        }

        stage('Docker Build') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE}:${IMAGE_TAG} ."
            }
        }

        stage('Docker Push') {
            when {
                branch 'main'
            }
            steps {
                sh "docker push ${DOCKER_IMAGE}:${IMAGE_TAG}"
            }
        }
    }
}
