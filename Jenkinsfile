pipeline {
    agent any

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
                setGitHubStatus('success', 'All checks passed')
            }
        }
        failure {
            script {
                // Установка статуса commit в GitHub как 'failure'
                setGitHubStatus('failure', 'Tests failed')
            }
        }
    }
}


def setGitHubStatus(String state, String description) {
    def commitSha = sh(script: 'git rev-parse HEAD', returnStdout: true).trim()
    def repoName = env.GIT_URL.split('/').dropRight(1).last().replaceAll('.git', '')
    def apiUrl = "https://api.github.com/repos/${repoName}/statuses/${commitSha}"

    withCredentials([string(credentialsId: 'github-token', variable: 'GITHUB_TOKEN')]) {
        sh """curl -X POST ${apiUrl} \
              -H "Authorization: token ${GITHUB_TOKEN}" \
              -d '{"state": "${state}", "context": "CI Tests", "description": "${description}", "target_url": "${env.BUILD_URL}"}'"""
    }
}
