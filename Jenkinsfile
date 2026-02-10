pipeline {
    agent any

    environment {
        COMPOSE_PROJECT_NAME = "hrms"
    }

    stages {

        stage('Build Docker Images') {
            steps {
                sh 'docker-compose build --no-cache'
            }
        }

        stage('Stop Old Containers') {
            steps {
                sh '''
                docker-compose down --volumes --remove-orphans || true
                '''
            }
        }

        stage('Start Containers') {
            steps {
                sh 'docker-compose up -d'
            }
        }

        stage('Wait for Database') {
            steps {
                sh '''
                echo "Waiting for MySQL to be ready..."
                until docker exec hrms-portal-db mysqladmin ping -h"localhost" --silent; do
                    echo "Database is not ready yet..."
                    sleep 5
                done
                echo "Database is ready!"
                '''
            }
        }

        stage('Run Migrations') {
            steps {
                sh 'docker exec hrms-portal-backend python manage.py migrate'
            }
        }

        stage('Collect Static Files') {
            steps {
                sh 'docker exec hrms-portal-backend python manage.py collectstatic --noinput'
            }
        }
    }

    post {
        success {
            echo 'HRMS Deployment Successful'
        }
        failure {
            echo 'HRMS Deployment Failed'
        }
    }
}

