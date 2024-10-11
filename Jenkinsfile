pipeline {
    agent any

    triggers {
        githubPush()
    }

    environment {
        GIT_REPO = 'git@github.com:neevink/jenkins-lesson.git'
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
        always {
            cleanWs()
        }
    }
}
