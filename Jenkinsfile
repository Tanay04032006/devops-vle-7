pipeline {
  agent any

  environment {
    KUBECONFIG = '/var/lib/jenkins/.kube/config'
    MINIKUBE_HOME = '/var/lib/jenkins'
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'docker build -t vle7-app:latest ./app'
      }
    }

    stage('Load Image into Minikube') {
      steps {
        sh 'minikube image load vle7-app:latest'
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        sh 'kubectl apply -f k8s/deployment.yaml'
        sh 'kubectl apply -f k8s/service.yaml'
        sh 'kubectl rollout status deployment/devops-app --timeout=120s'
      }
    }

    stage('Verify') {
      steps {
        sh 'kubectl get deployments,pods,services -o wide'
      }
    }
  }
}
