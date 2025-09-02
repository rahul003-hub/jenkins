pipeline {
    agent any

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/rahul003-hub/jenkins.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("myapp:latest")
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    echo "Running the container..."
                    sh "docker run -d -p 5000:5000 --name myapp_container myapp:latest"
                }
            }
        }

        stage('Wait for App to Start') {
            steps {
                script {
                    echo "Waiting for Flask app to start..."
                    sleep(time: 10, unit: 'SECONDS')
                }
            }
        }

        stage('Test App') {
            steps {
                script {
                    echo "Testing app..."
                    sh "curl -v http://localhost:5000 || echo 'App not reachable'"
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    echo "Cleaning up container..."
                    sh "docker stop myapp_container || true"
                    sh "docker rm myapp_container || true"
                }
            }
        }
    }
}
