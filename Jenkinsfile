pipeline {
    agent any

    environment {
        GITHUB_TOKEN = credentials('github-web-hook')  // Сохраните ваш GitHub токен как креденшнл в Jenkins
        REPO_NAME = 'jenkins-lesson'
    }

    stages {
        stage('git gheckout') {
            steps {
                checkout scm
            }
        }

        stage('unit-test') {
            steps {
                sh 'python3 ./test_app.py'
            }
        }

        stage('smoke-test') {
            steps {
                sh 'python3 ./app.py 2 3'
            }
        }
    }

    post {
        success {
            script {
                // Установка статуса commit в GitHub как 'success'
                def commitSha = sh(script: 'git rev-parse HEAD', returnStdout: true).trim()
                sh """curl -X POST https://api.github.com/repos/${env.REPO_NAME}/statuses/${commitSha} \
                      -H "Authorization: token ${env.GITHUB_TOKEN}" \
                      -d '{"state": "success", "context": "CI Tests", "description": "All checks passed", "target_url": "${env.BUILD_URL}"}'"""
            }
        }
        failure {
            script {
                // Установка статуса commit в GitHub как 'failure'
                def commitSha = sh(script: 'git rev-parse HEAD', returnStdout: true).trim()
                sh """curl -X POST https://api.github.com/repos/${env.REPO_NAME}/statuses/${commitSha} \
                      -H "Authorization: token ${env.GITHUB_TOKEN}" \
                      -d '{"state": "failure", "context": "CI Tests", "description": "Checks failed", "target_url": "${env.BUILD_URL}"}'"""
            }
        }
    }
}
