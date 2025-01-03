pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'snehcreate/expensetracker_v3'  // Docker image for the Spring Boot app
        DOCKER_TAG = 'latest'
        DOCKER_REGISTRY = 'docker.io'
        DOCKER_CREDENTIALS = 'docker-cred'  // Docker credentials ID in Jenkins
    }

    stages {
        stage("Checkout") {
            steps {
                script {
                    echo "Cloning the repository..."
                    clone("https://github.com/bodkekarbalaji95/Expenses-Tracker-Web_Application.git", "main")
                }
            }
        }

        stage("Build Docker Image") {
            steps {
                script {
                    echo "Building Docker image..."
                    dockerbuild(DOCKER_IMAGE, DOCKER_TAG)
                }
            }
        }

        stage("Push to DockerHub") {
            steps {
                script {
                    echo "Pushing Docker image to DockerHub..."
                    dockerpush(DOCKER_CREDENTIALS, DOCKER_IMAGE, DOCKER_TAG)
                }
            }
        }

        stage("Deploy Application") {
            steps {
                script {
                    echo "Deploying application..."
                    deploy()
                }
            }
        }

        stage("Clean Up") {
            steps {
                script {
                    echo "Cleaning up Docker resources..."
                    sh 'docker-compose down || true'
                    sh "docker rmi ${DOCKER_IMAGE}:${DOCKER_TAG} || true"
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline execution completed. Cleaning up workspace..."
            cleanWs()
        }
    }
}
