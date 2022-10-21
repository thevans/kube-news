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
            environment {
                tag_version = "${env.BUILD_ID}"
            }

            steps {
                withKubeConfig([credentialsId: 'kubeconfig']) {
                    sh 'sed -i "s/{{TAG}}/$tag_version/g" ./src/k8s/deployment.yaml'
                    sh 'kubectl apply -f ./src/k8s/deployment.yaml'
                }
            }
        }
    }
}