pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'sajat-felhasznalonev/python-app'
        DOCKER_TAG = 'latest'
        DOCKER_CREDENTIALS_ID = 'docker-hub-credentials-id'
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/sajat-repo/python-project.git'
            }
        }
        stage('Linting') {
            steps {
                sh 'pip install flake8'
                sh 'flake8 . --count --exit-zero --max-complexity=10 --max-line-length=127 --statistics'
            }
        }
        stage('Testing') {
            steps {
                sh 'pip install pytest'
                sh 'pytest tests/'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:$DOCKER_TAG .'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: "$DOCKER_CREDENTIALS_ID", variable: 'DOCKER_PASSWORD')]) {
                    sh 'echo $DOCKER_PASSWORD | docker login -u "sajat-felhasznalonev" --password-stdin'
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                sh 'docker push $DOCKER_IMAGE:$DOCKER_TAG'
            }
        }
    }
}
