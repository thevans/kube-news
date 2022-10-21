pipeline {
    agent any

    stages {
        stage ('Pull & Build docker image') {
            steps {
                script {
                    dockerapp = docker.build("thevansvilella/kube-news:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
                }
            }
        }

        stage ('Push docker image (CI)') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        dockerapp.push('latest')
                        dockerapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

        stage ('Deploy kubernetes (CD)') {
            steps {
                withKubeconfig([credentialsId: 'kubeconfig']) {
                    sh 'kubectl apply -f ./k8s/deployment.yaml'
                }
            }
        }
    }
}