pipeline {
  agent {
    docker {
      image 'bodkekarbalaji95/maven-balaji-docker-agent:v1'
      args '--user root -v /var/run/docker.sock:/var/run/docker.sock'  // This mounts the Docker socket
    }
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/bodkekarbalaji95/Expenses-Tracker-Web_Application.git'
      }
    }

    stage('Build and Test') {
      steps {
        sh 'mvn clean package'
      }
    }

    stage('Build and Push Docker Image') {
      environment {
        DOCKER_IMAGE = "bodkekarbalaji95/expenses_tracker:${BUILD_NUMBER}"
        REGISTRY_CREDENTIALS = credentials('docker-cred')
      }
      steps {
        script {
          sh 'cd java-maven-sonar-argocd-helm-k8s/spring-boot-app && docker build -t ${DOCKER_IMAGE} .'
          def dockerImage = docker.image("${DOCKER_IMAGE}")
          docker.withRegistry('https://index.docker.io/v1/', "docker-cred") {
            dockerImage.push()
          }
        }
      }
    }
  }

  post {
    always {
      cleanWs()  // This step is inside the post block, which should be valid now.
    }
  }
}
