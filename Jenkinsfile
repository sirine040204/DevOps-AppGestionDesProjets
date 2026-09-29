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
                sh '''
                    echo "Waiting for backend to start..."

                    for i in {1..30}; do
                        if curl -s -f http://localhost:8081/equipe/all > /dev/null; then
                            echo "Backend is ready!"
                            exit 0
                        fi

                        echo "Backend not ready yet... waiting 2 seconds"
                        sleep 2
                    done

                    echo "Backend failed to become ready."
                    docker compose logs backend
                    exit 1
                '''
            }
        }
    }

    post {
        always {
            sh 'docker compose logs --tail=50 || true'
        }
    }
}

