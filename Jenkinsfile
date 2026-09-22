pipeline {
    agent any
    stages{
        stage('checkout'){
            steps{checkout scm}
        }
        stage('Start Environment'){
            steps{
                sh 'docker compose up -d --build'
                sleep 3
            }
        }
        stage('Run Tests'){
            steps{
                sh 'docker compose exec -T web pytest test_app.py'
            }
        }
    }
    post{
        always{
            sh 'docker compose down'
        }
    }
}