pipeline {
  agent any
  
  environment {
     DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials') 
     IMAGE_NAME = "yidgar11/rmqp-example" // Replace with your Docker Hub username and desired image name
     IMAGE_TAG = "${env.BUILD_NUMBER}"
  }

  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/yidgar11/rmqp-example.git'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
      }
    }

    stage('Login to Docker Hub') {
      steps {
        sh "docker login -u ${DOCKERHUB_USERNAME} -p ${DOCKERHUB_PASSWORD}"
      }
    }

    stage('Push Docker Image') {
      steps {
        sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
      }
    }
  }

  post {
    success {
      echo 'Pipeline completed successfully!'
    }
    failure {
      echo 'Pipeline failed!'
    }
  }
}
