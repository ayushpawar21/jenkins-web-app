pipeline {
    agent any

    environment {
        COMPOSE_FILE = 'docker-compose.yml' // Adjust if your docker-compose file has a different name or path
    }

    stages {
        stage('Shut down existing services') {
            steps {
                script {
                    echo 'Stopping existing Docker Compose services...'
                    sh 'docker-compose down'
                }
            }
        }

        stage('Start Docker Compose services') {
            steps {
                script {
                    echo 'Starting Docker Compose services...'
                    sh 'docker-compose up -d --build'
                }
            }
        }

        stage('Check URL response time') {
            steps {
                script {
                    echo 'Checking URL response time...'
                    def url = "http://localhost:80"
                    def maxTime = 10 // Maximum allowed response time in milliseconds

                    // Using curl to measure response time
                    def responseTime = sh(
                        script: "curl -o /dev/null -s -w '%{time_total}' ${url}",
                        returnStdout: true
                    ).trim().toFloat() * 1000 // Convert to milliseconds

                    echo "Response time: ${responseTime} ms"

                    if (responseTime > maxTime) {
                        error "Response time exceeds ${maxTime} ms: ${responseTime} ms"
                    } else {
                        echo "Response time is within the limit: ${responseTime} ms"
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
            // Optionally, shut down services after the pipeline completes
             sh 'docker ps'
        }
    }
}
