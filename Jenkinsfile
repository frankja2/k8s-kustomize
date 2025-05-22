pipeline {
  agent { label 'terraform' }
  environment {
    KUBECONFIG_CRED_ID = 'k3s-kubeconfig'
  }
  stages {
    stage('Apply Kustomize') {
      steps {
        withCredentials([file(credentialsId: env.KUBECONFIG_CRED_ID, variable: 'KUBECONFIG')]) {
          sh '''
            export KUBECONFIG=$KUBECONFIG
            kubectl apply -k dev
            kubectl apply -k prod
          '''
        }
      }
    }
  }
}
