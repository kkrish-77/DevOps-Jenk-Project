pipeline {
    agent {
        docker {
            image 'docker:latest'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }

    environment {
        APP_NAME = 'flask-app'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/kkrish-77/DevOps-Jenk-Project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $APP_NAME .'
            }
        }

        stage('Run Container (Optional)') {
            steps {
                sh 'docker run -d --name ${APP_NAME} -p 5000:5000 $APP_NAME'
            }
        }
    }
}
