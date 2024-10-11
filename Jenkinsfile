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
        success {
            script {
                updateGitHubCommitStatus(currentBuild, 'success')
            }
           }
           failure {
               script {
                   updateGitHubCommitStatus(currentBuild, 'failure')
               }
           }
       }
   }

   def updateGitHubCommitStatus(currentBuild, status) {
       def commitSha = env.GIT_COMMIT ?: env.GIT_COMMIT
       def context = 'CI Tests'
       def description = (status == 'success') ? 'All checks passed' : 'Checks failed'
       def targetUrl = "${env.BUILD_URL}"

       step([
           $class: 'GitHubCommitStatusSetter',
           contextSource: [$class: 'ManuallyEnteredCommitContextSource', context: context],
           commitShaSource: [$class: 'ManuallyEnteredShaSource', sha: commitSha],
           errorHandlers: [[$class: 'ShallowAnyErrorHandler']],
           statusResultSource: [$class: 'ConditionalStatusResultSource', results: [
               [$class: 'AnyBuildResult', state: status, message: description, url: targetUrl]
           ]]
       ])
   }
