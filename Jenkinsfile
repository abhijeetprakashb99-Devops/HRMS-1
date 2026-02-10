pipeline {
    agent any

    environment {
        COMPOSE_PROJECT_NAME = "hrms"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/YOUR_USERNAME/YOUR_REPO.git'
            }
        }

        stage('Build Docker Images') {
            steps {
                sh 'docker-compose build --no-cache'
            }
        }

        stage('Stop Old Containers') {
            steps {
                sh 'docker-compose down'
            }
        }

        stage('Start Containers') {
            steps {
                sh 'docker-compose up -d'
            }
        }

        stage('Run Migrations') {
            steps {
                sh 'docker exec hrms_backend python manage.py migrate'
            }
        }

        stage('Collect Static Files') {
            steps {
                sh 'docker exec hrms_backend python manage.py collectstatic --noinput'
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful'
        }
        failure {
            echo 'Deployment Failed'
        }
    }
}

