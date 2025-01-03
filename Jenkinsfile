pipeline {
    agent any
    environment {
        DOCKER_CRED = 'docker-cred' // Using the same credential for Git and Docker
    }
    stages {
        stage('Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/bodkekarbalaji95/Expenses-Tracker-Web_Application.git',
                        credentialsId: "${DOCKER_CRED}"
                    ]]
                ])
            }
        }
        stage('Build Docker Image') {
            steps {
                echo "Building Docker Image..."
                sh "docker build -t your-image:tag ."
            }
        }
        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "${DOCKER_CRED}",
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh """
                    echo "${DOCKER_PASS}" | docker login -u "${DOCKER_USER}" --password-stdin
                    docker push your-image:tag
                    """
                }
            }
        }
        stage('Deploy Application') {
            steps {
                echo "Deploying application..."
                // Add deployment steps here
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
