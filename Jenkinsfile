pipeline {
    agent any

    environment {
        TELEGRAM_TOKEN = credentials('tgtoken')
        TELEGRAM_CHAT_ID = credentials('tgchatid')

        TEXT_PRE_BUILD = "Jenkins is building ${JOB_NAME}"
        TEXT_SUCCESS = "${JOB_NAME} is Success"
        TEXT_FAILURE = "${JOB_NAME} is Failure"
    }

    stages {
        stage('Notification'){
            steps {
                sh "curl -s -X POST https://api.telegram.org/bot${TELEGRAM_TOKEN}/sendMessage -d chat_id=${TELEGRAM_CHAT_ID} -d text='${TEXT_PRE_BUILD}'"
            }
        }
        stage('Check'){
            steps {
                echo 'Checking files'
                sh 'ls -l index.html Dockerfile'
            }
        }
        stage('Delete') {
            steps {
                sh 'docker rm -f test_1 || true'
            }
        }
        stage('Build image') {
            steps {
                sh 'docker build -t lab2:latest .'
                sh 'docker tag lab2 torrros/lab2:latest'
                sh "docker tag lab2 torrros/lab2:${env.BUILD_NUMBER}"
            }
        }
        stage('Push to Docker Hub') {
            steps {
                withDockerRegistry([ credentialsId: "lab", url: "" ]) {
                    sh "docker push torrros/lab2:latest"
                    sh "docker push torrros/lab2:${env.BUILD_NUMBER}"
                }
            }
        }
    } // Ця дужка закриває блок stages

    post {
        success {
            sh "curl -s -X POST https://api.telegram.org/bot${TELEGRAM_TOKEN}/sendMessage -d chat_id=${TELEGRAM_CHAT_ID} -d text='${TEXT_SUCCESS}'"
        }
        failure {
            sh "curl -s -X POST https://api.telegram.org/bot${TELEGRAM_TOKEN}/sendMessage -d chat_id=${TELEGRAM_CHAT_ID} -d text='${TEXT_FAILURE}'"
        }
    }
}
