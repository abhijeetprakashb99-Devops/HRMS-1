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
                docker rm -f hrms-portal-backend || true
                docker rm -f hrms-portal-db || true
                docker rm -f hrms-portal-redis || true
                '''
            }
        }

        stage('Start Containers') {
            steps {
                sh 'docker-compose up -d'
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

