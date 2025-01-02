pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'snehcreate/expensetracker_v3'  // Docker image for your Spring Boot app
        DOCKER_TAG = 'latest'
        DOCKER_REGISTRY = 'docker.io'
        DOCKER_CREDENTIALS = 'docker-credentials'  // Replace with your Jenkins Docker credentials ID
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from your repository
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image for the Spring Boot application
                    sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
                }
            }
        }

        stage('Docker Compose Up') {
            steps {
                script {
                    // Run Docker Compose to start both the Spring Boot app and MySQL container
                    sh 'docker-compose up -d'
                }
            }
        }

        stage('Login to Docker Registry') {
            steps {
                script {
                    // Login to Docker registry using the credentials
                    withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh "echo ${DOCKER_PASSWORD} | docker login -u ${DOCKER_USERNAME} --password-stdin ${DOCKER_REGISTRY}"
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Push the Docker image to the registry
                    sh "docker push ${DOCKER_IMAGE}:${DOCKER_TAG}"
                }
            }
        }
        
        stage('Clean up Docker Containers') {
            steps {
                script {
                    // Clean up the containers to avoid buildup
                    sh 'docker-compose down'
                }
            }
        }
    }

    post {
        always {
            // Remove the image locally after build and push
            sh "docker rmi ${DOCKER_IMAGE}:${DOCKER_TAG} || true"
        }
    }
}