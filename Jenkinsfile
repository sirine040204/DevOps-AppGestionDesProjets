pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Images') {
            steps {
                sh 'docker compose build'
            }
        }

        stage('Start Application') {
            steps {
                sh 'docker compose up -d'
            }
        }

        stage('Check Containers') {
            steps {
                sh 'docker compose ps'
            }
        }

        stage('Test Backend') {
            steps {
                sh 'curl -f http://localhost:8081/equipe/all'
            }
        }
    }

    post {
        always {
            sh 'docker compose logs --tail=50 || true'
        }
    }
}
