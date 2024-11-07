pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = 'gauravsadhwani'  // Replace with your Docker Hub username
        DOCKERHUB_REPO = 'gaurav_assessment'    // Replace with your Docker Hub repository name
        DOCKER_IMAGE_TAG = "${DOCKERHUB_USERNAME}/${DOCKERHUB_REPO}:latest"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    // Install dependencies (Node.js)
					sh 'cd app'
					sh 'ls'
                    sh 'yarn install'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image and tag it
					sh 'ls'
                    dockerImage = docker.build("${DOCKER_IMAGE_TAG}")
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    // Log in to Docker Hub
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials-id') {
                        // Push the Docker image to Docker Hub
                        dockerImage.push("latest")
                    }
                }
            }
        }

    }

    post {
        always {
            echo 'Cleaning up...'
            sh 'docker rmi ${DOCKER_IMAGE_TAG} || true'  // Clean up the Docker image from Jenkins agent
        }
    }
}
