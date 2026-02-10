pipeline {
    agent any

    environment {
        COMPOSE_PROJECT_NAME = "hrms"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/abhijeetprakashb99-Devops/HRMS-1.git'
            }
        }

        stage('Build Docker Images') {
            steps {
                sh 'docker-compose build --no-cache'
            }
        }

        stage('Clean Old Containers') {
    	    steps {
        	sh '''
        	docker rm -f hrms-portal-db || true
        	docker rm -f hrms-portal-redis || true
        	docker rm -f hrms_backend || true
        	docker-compose down || true
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

