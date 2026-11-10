pipeline {
    agent any

    environment {
        IMAGE = "vle7-cicd-demo:latest"
        KUBECONFIG = "/etc/rancher/k3s/k3s.yaml"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE .'
            }
        }

        stage('Import Image into K3s') {
            steps {
                sh 'docker save $IMAGE | sudo -n /usr/local/bin/k3s ctr images import -'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '/usr/local/bin/kubectl apply -f k8s/'
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '/usr/local/bin/kubectl rollout status deployment/vle7-app --timeout=120s'
                sh '/usr/local/bin/kubectl get pods,services'
            }
        }
    }
}
