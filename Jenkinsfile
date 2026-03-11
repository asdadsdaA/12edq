pipeline {
    agent any

    environment {
        // 🔸 Укажите правильное имя образа, включая ваш логин Docker Hub.
        //    Убедитесь, что 'bnmf' - это правильный логин.
        DOCKER_IMAGE = 'bnmf/my-php-app'
        
        // 🔸 Замените '123457' на реальный ID ваших учетных данных Docker Hub из Jenkins.
        DOCKER_CREDENTIALS_ID = '123457'
    }

    stages {
        stage('Checkout') {
            steps {
                // Клонируем репозиторий
                git branch: 'main', url: 'https://github.com/asdadsdaA/12edq.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Собираем Docker-образ и тегируем его номером сборки
                    dockerImage = docker.build("${env.DOCKER_IMAGE}:${env.BUILD_ID}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Авторизуемся в Docker Hub, используя учетные данные
                    docker.withRegistry('https://index.docker.io/v1/', env.DOCKER_CREDENTIALS_ID) {
                        
                        // Пушим образ с тегом - номером сборки
                        dockerImage.push() // Тег уже был присвоен на этапе сборки

                        // Добавляем тег 'latest' и также пушим его
                        dockerImage.push('latest')
                    }
                }
            }
        }
    }

    post {
        // Этот блок выполняется всегда, независимо от результата
        always {
            echo 'Очистка неиспользуемых Docker-образов...'
            sh 'docker image prune -f'
        }
        // Этот блок выполняется только при успешном завершении пайплайна
        success {
            echo '✅ Пайплайн успешно завершен!'
        }
        // Этот блок выполняется только при ошибке в пайплайне
        failure {
            echo '❌ В пайплайне произошла ошибка!'
        }
    }
}
