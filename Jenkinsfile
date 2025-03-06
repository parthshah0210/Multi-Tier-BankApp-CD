pipeline {
    agent any

    stages {
        stage('git chekout') {
            steps {
                git branch: 'main', url: 'https://github.com/parthshah0210/Multi-Tier-BankApp-CD.git'
            }
        }
        stage('deployment') {
            steps {
                withKubeConfig(caCertificate: '', clusterName: 'minikube', contextName: 'minikube', credentialsId: 'k8s-cred', namespace: 'bankapp', restrictKubeConfigAccess: false, serverUrl: 'https://192.168.49.2:8443') {
                    sh 'kubectl apply -f bankapp/bankapp-ds.yml -n bankapp --validate=false --insecure-skip-tls-verify'
                    sh 'kubectl apply -f database/database-ds.yml -n bankapp --validate=false --insecure-skip-tls-verify'
                    sleep 30
                }
            }
        }
        stage('verify deployment') {
            steps {
                withKubeConfig(caCertificate: '', clusterName: 'minikube', contextName: 'minikube', credentialsId: 'k8s-cred', namespace: 'bankapp', restrictKubeConfigAccess: false, serverUrl: 'https://192.168.49.2:8443') {
                    sh 'kubectl get pods -n bankapp'
                    sh 'kubectl get svc -n bankapp'
                }
            }
        }
    }
}
