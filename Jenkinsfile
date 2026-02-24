pipeline {
    agent any
    stages {
        stage('1') {
            steps {
                sh 'docker rm -f test_1 || true'
            }
        }
        stage('Build nginx/custom') {
            steps {
                sh 'docker build -t nginx/custom:latest .'
            }
        }
        stage('Test nginx/custom') {
            steps {
                echo 'Pass'
            }
        }
        stage('Deploy nginx/custom'){
            steps{
                sh "docker run -d --name test_1 -p 80:80 nginx/custom:latest"
            }
        }
    }
}
