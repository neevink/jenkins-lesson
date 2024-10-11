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
                sh 'which pip3 || true'
                sh 'which pip || true'
                sh 'which python || true'
                sh 'which python3 || true'
            }
        }

        stage('Lint') {
            steps {
                // Запускаем линтер flake8
                // sh 'flake8 .'
                echo 'lint'
            }
        }

        stage('Test') {
            steps {
                // Запускаем тесты с помощью pytest
                // sh 'pytest'
                echo 'test'
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
