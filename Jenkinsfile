pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Забираем код из системы контроля версий
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                // Устанавливаем зависимости
                // sh 'pip install flake8 pytest'
                sh 'ls -la'
            }
        }

        stage('Lint') {
            steps {
                // Запускаем линтер flake8
                // sh 'flake8 .'
            }
        }

        stage('Test') {
            steps {
                // Запускаем тесты с помощью pytest
                // sh 'pytest'
            }
        }
    }

    post {
        always {
            // Убираем временные файлы
            cleanWs()
        }
    }
}
