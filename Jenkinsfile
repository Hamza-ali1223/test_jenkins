pipeline {
  agent any

  environment {
    IMAGE_NAME = 'test_jenkins_nginx'
    CONTAINER_NAME = 'nginx_static_site'
    PORT = '8080'
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/Hamza-ali1223/test_jenkins.git'
      }
    }

    stage('Build Docker Image') {
      steps {
        echo "Building Docker image..."
        bat "docker build -t ${IMAGE_NAME} ."
      }
    }

    stage('Stop Old Container') {
      steps {
        echo "Stopping old container if exists..."
        bat """
          docker ps -q --filter "name=${CONTAINER_NAME}" | findstr . && docker stop ${CONTAINER_NAME} && docker rm ${CONTAINER_NAME} || echo No old container
        """
      }
    }

    stage('Run New Container') {
      steps {
        echo "Running new container..."
        bat "docker run -d -p ${PORT}:80 --name ${CONTAINER_NAME} ${IMAGE_NAME}"
      }
    }

    stage('Verify Running Containers') {
      steps {
        bat "docker ps"
      }
    }
  }

  post {
    success {
      echo "✅ Deployed successfully! Visit: http://localhost:${PORT}"
    }
    failure {
      echo "❌ Deployment failed. Check logs."
    }
  }
}
