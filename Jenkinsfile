pipeline {
    agent any

    environment {
        REGISTRY = "your-docker-registry"
        REPO = "my-web-app"
        DOCKER_CREDENTIALS_ID = "docker-hub-credentials"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                script {
                    docker.build("${env.REGISTRY}/${env.REPO}:latest")
                }
            }
        }
        stage('Test') {
            steps {
                sh 'npm test' // Add your test script here
            }
        }
        stage('Push') {
            steps {
                script {
                    docker.withRegistry('', "${env.DOCKER_CREDENTIALS_ID}") {
                        docker.image("${env.REGISTRY}/${env.REPO}:latest").push()
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // Example deployment script
                    sh '''
                    docker pull ${env.REGISTRY}/${env.REPO}:latest
                    docker stop my_app || true
                    docker rm my_app || true
                    docker run -d -p 8080:8080 --name my_app ${env.REGISTRY}/${env.REPO}:latest
                    '''
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}

