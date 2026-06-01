pipeline {
    agent any
    environment {
        PROJECT_DIR = "/workspace/proshop-v2"
    }
    stages {
        stage('Informations') {
            steps {
                echo '===== Informations ====='
                sh 'pwd && ls -la $PROJECT_DIR'
            }
        }
        stage('Docker Test') {
            steps {
                echo '===== Verification Docker ====='
                sh 'docker --version && docker compose version && docker ps'
            }
        }
        stage('Build Backend') {
            steps {
                echo '===== Build Backend ====='
                sh 'cd $PROJECT_DIR && docker build -t proshop-backend:latest -f backend/Dockerfile .'
            }
        }
        stage('Build Frontend') {
            steps {
                echo '===== Build Frontend ====='
                sh 'cd $PROJECT_DIR && docker build -t proshop-frontend:latest frontend/'
            }
        }
        stage('Deploy') {
            steps {
                echo '===== Deploiement ====='
                sh '''
                    cd $PROJECT_DIR
                    docker compose down || true
                    docker compose up -d --build
                '''
            }
        }
        stage('Verify') {
            steps {
                echo '===== Verification ====='
                sh 'cd $PROJECT_DIR && docker ps && docker compose ps'
            }
        }
    }
    post {
        success {
            echo 'PIPELINE EXECUTE AVEC SUCCES'
            echo 'Application : http://localhost'
        }
        failure {
            echo 'ECHEC DU PIPELINE'
        }
    }
}