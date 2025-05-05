pipeline {
    agent any

    environment {
        APP_NAME = 'flask-app'
        PORT = '5000'
    }

    stages {
        stage('Clone') {
            steps {
                cleanWs()
                git branch: 'main',
                    url: 'https://github.com/kkrish-77/DevOps-Jenk-Project.git'
            }
        }

        stage('Build') {
            steps {
                sh '''
                sudo chmod 666 /var/run/docker.sock || true
                
                docker build -t ${APP_NAME} .
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker stop ${APP_NAME} || true
                docker rm ${APP_NAME} || true
                
                docker run -d --name ${APP_NAME} \
                    -p ${PORT}:${PORT} \
                    --restart unless-stopped \
                    ${APP_NAME}
                
                docker ps | grep ${APP_NAME}
                
                echo "Application deployed successfully!"
                '''
            }
        }
    }

    post {
        success {
            echo "Application is running at http://localhost:${PORT}"
        }
        failure {
            sh '''
            docker stop ${APP_NAME} || true
            docker rm ${APP_NAME} || true
            '''
            echo 'Deployment failed - cleaned up containers'
        }
    }
}
