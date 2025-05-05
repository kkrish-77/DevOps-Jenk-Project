pipeline {
    agent any

    environment {
        IMAGE_NAME = 'flask-app-local'
    }

    stages {
        stage('Clone') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/kkrish-77/DevOps-Jenk-Project.git'
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t flask-app .'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker rm -f flask-app || true
                docker run -d -p 5000:5000 --name flask-app flask-app
                echo "Application deployed successfully!"
                '''
            }
        }
    }

    post {
        success {
            echo 'Visit http://localhost:5000 to see your application'
        }
        failure {
            script {
                if (isUnix()) {
                    sh 'docker rm -f flask-app || true'
                } else {
                    bat 'docker rm -f flask-app || true'
                }
            }
            echo 'Pipeline failed! Cleaned up containers.'
        }
    }
}
