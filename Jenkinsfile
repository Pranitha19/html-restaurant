pipeline {
    agent any

    environment {
        IMAGE_NAME = 'my-app'
        DOCKER_REGISTRY = 'dockerhub_username'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/user/repo.git'
            }
        }

        stage('Build Application') {
            steps {
                sh 'mvn clean package' // For Java Apps
                // sh 'npm install && npm run build' // For Node.js Apps
            }
        }

        stage('Run Tests') {
            steps {
                sh 'mvn test' // Run unit tests
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_REGISTRY/$IMAGE_NAME:$BUILD_NUMBER .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withDockerRegistry([credentialsId: 'docker-hub-credentials', url: '']) {
                    sh 'docker push $DOCKER_REGISTRY/$IMAGE_NAME:$BUILD_NUMBER'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/deployment.yaml'
            }
        }
    }

    post {
        success {
            echo "Deployment Successful!"
        }
        failure {
            echo "Build Failed!"
        }
    }
}
