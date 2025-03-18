pipeline {
    agent any

    environment {
        DOCKER_HUB_USER = "yphalak"
        DOCKER_HUB_PASS = credentials('docker-hub-credentials')  // Add your DockerHub credentials in Jenkins
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/YPhalak/jenkins-k8s-pipeline.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_HUB_USER/flask-k8s:latest .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                sh 'docker login -u $DOCKER_HUB_USER -p $DOCKER_HUB_PASS'
                sh 'docker push $DOCKER_HUB_USER/flask-k8s:latest'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }
    }

    post {
        success {
            echo "✅ Deployment Successful!"
        }
        failure {
            echo "❌ Deployment Failed!"
        }
    }
}

