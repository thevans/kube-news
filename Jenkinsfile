pipeline {
    agent any

    stages {
        stage ('Build docker image') {
            steps {
                scripts {
                    dockerapp = docker.Build('thevansvilella/kube-news:${env.BUILD_ID}', '-f ./src/Dockerfile ./src')
                }
            }
        }
    }
}