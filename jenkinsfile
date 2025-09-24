pipeline {
    agent none
    stages {
        stage('Echo Branch Info') {
            agent any
            steps {
                echo "Hello from branch ${env.BRANCH_NAME}"
            }
        }
        
        stage('Build on Instance Agent') {
            agent { label 'ec2-agent' }
            steps {
                checkout scm
                sh 'docker build -t app-instance:latest .'
                sh 'docker stop app-instance || true'
                sh 'docker rm app-instance || true'
                sh 'docker run -d --rm --name app-instance -p 3001:3000 app-instance:latest'
            }
        }
        
        stage('Build on Docker Agent') {
            agent { label 'docker-agent' }
            steps {
                checkout scm
                sh 'docker build -t app:latest .'
                sh 'docker stop app-container || true'
                sh 'docker rm app-container || true'
                sh 'docker run -d --rm --name app-container -p 3000:3000 app:latest'
            }
        }
    }
}
