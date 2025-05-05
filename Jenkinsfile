pipeline {
    agent any

    environment {
        IMAGE_NAME = 'flask-app-local'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/kkrish-77/DevOps-Jenk-Project.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Run Container') {
            steps {
                // Stop old container if it exists
                sh 'docker rm -f flask-app || true'
                // Run new container
                sh 'docker run -d -p 5000:5000 --name flask-app $IMAGE_NAME'
            }
        }
    }

    post {
        success {
            echo 'App is running on http://localhost:5000'
        }
    }
}
