pipeline {
    agent any

    environment {
        dockerRegistry = 'https://index.docker.io/v1/'
        dockerCreds = credentials('docker_credentials')
        backendImage = 'mern-backend'
        frontendImage = 'mern-frontend'
    }

    stages {
        stage('Build Backend') {
            steps {
                script {
                    def backendPath = './backend'
                    if (fileExists(backendPath)) {
                        echo "Building backend image"
                        bat "docker build -t ${backendImage}:latest ${backendPath}" // Build the image
                        bat "docker tag ${backendImage}:latest anujpal1213145178/${backendImage}:latest" // Tag image
                    } else {
                        error "Backend directory not found"
                    }
                }
            }
        }

        stage('Build Frontend') {
            steps {
                script {
                    def frontendPath = './client'
                    if (fileExists(frontendPath)) {
                        echo "Building frontend image"
                        bat "docker build -t ${frontendImage}:latest ${frontendPath}" // Build the image
                        bat "docker tag ${frontendImage}:latest anujpal1213145178/${frontendImage}:latest" // Tag image
                    } else {
                        error "Frontend directory not found"
                    }
                }
            }
        }

        stage('Push Backend') {
            steps {
                script {
                    echo "Preparing to push backend image"
                    docker.withRegistry(dockerRegistry, dockerCreds) {
                        bat "docker push anujpal1213145178/${backendImage}:latest"
                    }
                }
            }
        }

        stage('Push Frontend') {
            steps {
                script {
                    echo "Preparing to push frontend image"
                    docker.withRegistry(dockerRegistry, dockerCreds) {
                        bat "docker push anujpal1213145178/${frontendImage}:latest"
                    }
                }
            }
        }
    }

    post {
        always {
            echo "PIPELINE SUCCESS"
        }
        failure {
            echo "PIPELINE FAILED"
        }
    }
}