pipeline {
    agent any

    tools {
        nodejs 'node-18' // configure this in Jenkins UI later
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: '/Users/pranshugupta/jenkins-docker/jenkins-docker/sample-node-app'
            }
        }

        stage('Build') {
            steps {
                script {
                    docker.image('node:18-alpine').inside {
                        sh 'npm install'
                    }
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    docker.image('node:18-alpine').inside {
                        sh 'npm test'
                    }
                }
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    sh 'docker build -t my-node-app:latest .'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    sh 'docker stop my-node-app-container || true'
                    sh 'docker rm my-node-app-container || true'
                    sh 'docker run -d --name my-node-app-container -p 3001:3000 my-node-app:latest'
                }
            }
        }
    }
}
