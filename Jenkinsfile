pipeline {
    agent any

    environment {
        IMAGE_NAME = "rioaakash/repu-pipeline"
        TAG = "latest"
    }

    stages {

        stage('Clone Code') {
            steps {
                git 'https://github.com/aakash-rio/repo-pipeline.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$TAG .'
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS')]) {

                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                }
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $IMAGE_NAME:$TAG'
            }
        }

        stage('Deploy Container') {
            steps {
                sh 'docker rm -f nodeapp || true'
                sh 'docker run -d -p 80:3000 --name nodeapp $IMAGE_NAME:$TAG'
            }
        }
    }
}
