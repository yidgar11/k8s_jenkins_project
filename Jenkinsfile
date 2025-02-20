pipeline {
    // agent {
    //     docker {
    //         image 'docker:latest'  // Or a specific Docker image with necessary tools
    //         label 'docker' // Ensure your Jenkins agent has the 'docker' label
    //         args '-v /var/run/docker.sock:/var/run/docker.sock' // Allow Docker-in-Docker
    //     }
    // }


    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials') 
        IMAGE_NAME = "yidgar11/rmqp-example" // Replace with your Docker Hub username and desired image name
        IMAGE_TAG = "${env.BUILD_NUMBER}"
    }

    stages {
        
        stage('Login to DockerHub') {
            steps {
                script {
                    // Extract username and password from stored credentials
                    def dockerCreds = DOCKERHUB_CREDENTIALS.split(":")
                    def username = dockerCreds[0]
                    def password = dockerCreds[1]
                    
                    // Login to DockerHub
                    sh "echo $password | docker login -u $username --password-stdin"
                }
            }
        }
    
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
