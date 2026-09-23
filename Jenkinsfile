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
        stage('Push to Docker Hub') {
            steps {
                // This securely injects your credentials as environment variables for this block only
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER')]) {
                    sh "echo \$DOCKER_PASS | docker login -u \$DOCKER_USER --password-stdin"
                    
                    // You must tag the image with your specific Docker Hub username before pushing
                    sh "docker tag jenkins-python-app:latest \$DOCKER_USER/jenkins-python-app:latest"
                    sh "docker push \$DOCKER_USER/jenkins-python-app:latest"
                }
            }
        }
    }
    post {
        always {
            sh 'docker compose down'
        }
        success {
            echo "✅ Pipeline completed successfully! Image pushed to Docker Hub."
            // In a real setup, this is where you trigger an email or webhook
        }
        failure {
            echo "❌ Pipeline failed! Developers have been notified."
        }
    }
}