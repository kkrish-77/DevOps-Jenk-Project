pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                // Checkout code from Codeberg repository
                checkout scm
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    def imageName = "flask-server-cicd"
                    sh "docker build -t ${imageName} ."
                }
            }
        }
        
        stage('Deploy Container') {
            steps {
                script {
                    def containerName = "flask-server-cicd"
                    // Stop and remove any existing container
                    sh "docker stop ${containerName} || true"
                    sh "docker rm ${containerName} || true"
                    // Run the new container
                    sh "docker run -d --name ${containerName} -p 5000:5000 flask-server-cicd"
                }
            }
        }
    }
    
    post {
        success {
            echo 'Server deployed in Docker container successfully!'
        }
        failure {
            echo 'Docker deployment failed!'
        }
    }
} 