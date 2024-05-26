pipeline {
    agent any

    stages {
        stage('Docker Compose Down') {
            steps {
                script {
                    // Bring down any existing Docker Compose setup
                    sh 'docker compose down'
                }
            }
        }
        stage('Docker Compose Up') {
            steps {
                script {
                    // Bring up the Docker Compose setup
                    sh 'docker compose up -d --build'
                }
            }
        }
        stage('Check Service') {
            steps {
                script {
                    // Wait for a few seconds to ensure the service is up
                    sleep 10

                    // Check if the service is accessible and returns HTTP status 200
                    def response = sh(script: "curl -s -o /dev/null -w '%{http_code}' http://localhost:80", returnStdout: true).trim()
                    if (response != '200') {
                        error("Service not accessible. HTTP status code: ${response}")
                    }
                }
            }
        }
        stage('Docker Compose Status') {
            steps {
                script {
                    // Check the status of the Docker Compose services
                    sh 'docker compose ps'
                }
            }
        }
    }
}
