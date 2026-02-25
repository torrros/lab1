pipeline {
    agent any
    stages {
        stage('Delete') {
            steps {
                sh 'docker rm -f test_1 || true'
            }
        }
        stage('Build image') {
            steps {
                sh 'docker build -t lab2:latest .'
                sh 'docker tag lab2 torrros/lab2:latest'
                sh 'docker tag lab2 torrros/lab2:$BUILD_NUMBER'
            }
        }
        stage('Push') {
            steps {
                withDockerRegistry([ credentialsId: "torrros", url: "" ])

		sh 'docker tag lab2 torrros/lab2:latest'
                sh 'docker tag lab2 torrros/lab2:$BUILD_NUMBER'

            }
        }
        stage('Deploy nginx/custom'){
            steps{
                sh "docker run -d --name test_1 -p 80:80 torrros/lab2"
            }
        }
    }
}
