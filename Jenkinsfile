pipeline {
    agent any
    stages {
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
        stage('Deploy image') {
            steps {
                sh "docker run -d --name test_1 -p 80:80 torrros/lab2:latest"
            }
        }
    }
}
