pipeline {
    agent any

    environment {
        KUBECONFIG = '/var/lib/jenkins/.kube/config'
        MINIKUBE_HOME = '/var/lib/jenkins/.minikube'
    }

    stages {

        stage('Build') {
            steps {
                sh 'docker build -t myapp:v2 .'
            }
        }

        stage('Load Image into Minikube') {
            steps {
                sh 'minikube image load myapp:v2'
            }
        }

        stage('Deploy Green') {
            steps {
                sh 'kubectl apply -f deployment-green.yaml'
            }
        }

        stage('Verify Green') {
            steps {
                sh 'kubectl rollout status deployment/myapp-green'
            }
        }

        stage('Switch Traffic') {
            steps {
                input message: 'Switch traffic to Green?', ok: 'Switch to Green'
                sh '''kubectl patch service myapp-service -p '{"spec":{"selector":{"app":"myapp","color":"green"}}}' '''
            }
        }
    }
}
