pipeline {
    agent any
    environment {
        K8S_PORT = 51971
    }
    stages {
        stage('Deploy on MicroK8s') {
            when { 
                environment name: 'MicroK8s', value: 'true'
            }
            steps {
                sh "microk8s kubectl apply -f ./k8s/configmap.yaml"
                sh "microk8s kubectl apply -f ./k8s/credentials.yaml"
                sh "microk8s kubectl apply -f ./k8s/pv.yaml"
                sh "microk8s kubectl apply -f ./k8s/pvc.yaml"
                sh "microk8s kubectl apply -f ./k8s/deployment.yaml"
                sh "microk8s kubectl apply -f ./k8s/service.yaml"
            }
        }
        stage('Deploy on k8s') {
            steps {
                withCredentials([ string(credentialsId: 'minikube-credential', variable: 'api_token') ]) {
                    sh "kubectl --token $api_token --server https://host.docker.internal:${env.K8S_PORT}  --insecure-skip-tls-verify=true apply -f ./k8s/configmap.yaml"
                    sh "kubectl --token $api_token --server https://host.docker.internal:${env.K8S_PORT}  --insecure-skip-tls-verify=true apply -f ./k8s/credentials.yaml"
                    sh "kubectl --token $api_token --server https://host.docker.internal:${env.K8S_PORT}  --insecure-skip-tls-verify=true apply -f ./k8s/pv.yaml"
                    sh "kubectl --token $api_token --server https://host.docker.internal:${env.K8S_PORT}  --insecure-skip-tls-verify=true apply -f ./k8s/pvc.yaml"
                    sh "kubectl --token $api_token --server https://host.docker.internal:${env.K8S_PORT}  --insecure-skip-tls-verify=true apply -f ./k8s/deployment.yaml"
                    sh "kubectl --token $api_token --server https://host.docker.internal:${env.K8S_PORT}  --insecure-skip-tls-verify=true apply -f ./k8s/service.yaml"
                }
            }
        }
    }
}