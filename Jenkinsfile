pipeline {
    agent any
    
    environment {
        // 🔸 Замените на ваш логин Docker Hub
        DOCKER_IMAGE = 'bnmf/my-php-app'
        // 🔸 ID учётных данных, которые вы добавили в Jenkins
        DOCKER_CREDENTIALS_ID = '123457'
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/asdadsdaA/12edq.git'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${env.DOCKER_IMAGE}:${env.BUILD_ID}")
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', env.DOCKER_CREDENTIALS_ID) {
                        dockerImage.push("${env.BUILD_ID}")
                        dockerImage.push('latest')
                    }
                }
            }
        }
    }
    
    post {
        always {
            sh 'docker image prune -f'
        }
        success {
            echo '✅ Pipeline succeeded!'
        }
        failure {
            echo '❌ Pipeline failed!'
        }
    }
}
