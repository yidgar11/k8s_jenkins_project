pipeline {
  //agent any
  agent {
        docker {
            image 'docker:24.0.6-dind' // Use a Docker-in-Docker image
            args '--privileged'       // Required to enable Docker-in-Docker
        }
    }
  // agent {
  //     kubernetes {
  //       label 'my-docker-agent'  // all your pods will be named with this prefix, followed by a unique id
  //       idleMinutes 5  // how long the pod will live after no jobs have run on it
  //       yamlFile 'build-pod.yaml'  // path to the pod definition relative to the root of our project 
  //       defaultContainer 'docker'  // define a default container if more than a few stages use it, will default to jnlp container
  //     }
  // }

  environment {
     DOCKERHUB = credentials('dockerhub-credentials1') 
      // The credentials() function automatically creates:
        //DOCKERHUB_USR - username
        // DOCKERHUB_PSW - password
        // DOCKERHUB - username:password
     IMAGE_NAME = "yidgar11/rmqp-example" // Replace with your Docker Hub username and desired image name
     IMAGE_TAG = "${env.BUILD_NUMBER}"
  }

  stages {
    stage('Checkout github') {
      steps {
        git 'https://github.com/yidgar11/rmqp-example.git'
        sh 'ls -l' 
      }
    }

    stage('Build Docker Image') {
      steps {
        echo "going to build the docker image"
        //sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
      }
    }

   stage('Login to Docker Hub') {
            steps {
                sh '''
                echo $DOCKERHUB_PSW | docker login -u $DOCKERHUB_USR --password-stdin
                '''
            }
  }

    stage('Push Docker Image') {
      steps {
        echo "Pushing Docmer images" 
        //sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
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
