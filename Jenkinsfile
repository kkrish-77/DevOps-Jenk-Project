pipeline {
    agent any

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
