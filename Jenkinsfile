pipeline {
    agent any

    stages {

        stage ('Build Docker Image'){
            steps {
                script {
                    dockerapp= docker.build("thiagofh/kube-news:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
                }
            }
        }

        stage ('Push Docker Image'){
            steps{
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub'){
                        dockerapp.push('lateste')
                        dockerapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

        stage ('Deploy Kubernetes'){
            steps{
                script {
                    withKubeconfig([credentialsId: 'kubeconfig']){
                        sh 'kubectl -f ./k8s/deployment.yaml'
                    }
                }
            }
        }
    
    }
}