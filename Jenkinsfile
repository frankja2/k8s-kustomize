pipeline {
  agent { label 'terraform' }

  environment {
    KUBECONFIG_CRED_ID = 'k3s-kubeconfig'
  }

  stages {
    stage('Validate manifests (dev)') {
      steps {
        withCredentials([file(credentialsId: env.KUBECONFIG_CRED_ID, variable: 'KUBECONFIG')]) {
          sh '''
            export KUBECONFIG=$KUBECONFIG
            echo "🔍 Validating dev/ with kubeconform..."
        bash -c 'kubeconform -strict -summary -kubernetes-version 1.32.4 <(kubectl kustomize dev/)'
          '''
        }
      }
    }

    stage('Dry Run Kustomize Dev') {
      steps {
        withCredentials([file(credentialsId: env.KUBECONFIG_CRED_ID, variable: 'KUBECONFIG')]) {
          sh '''
            export KUBECONFIG=$KUBECONFIG
            echo "🔍 Dry run: dev/"
            kubectl apply --dry-run=client -k dev/
            kubeconform -h
          '''
        }
      }
    }

    stage('Apply Kustomize Dev') {
      steps {
        withCredentials([file(credentialsId: env.KUBECONFIG_CRED_ID, variable: 'KUBECONFIG')]) {
          sh '''
            export KUBECONFIG=$KUBECONFIG
            echo "🚀 Deploying: dev/"
            kubectl apply -k dev/
          '''
        }
      }
    }

    stage('Apply Kustomize Prod') {
      steps {
        withCredentials([file(credentialsId: env.KUBECONFIG_CRED_ID, variable: 'KUBECONFIG')]) {
          sh '''
            export KUBECONFIG=$KUBECONFIG
            echo "🚀 Deploying: prod/"
            kubectl apply -k prod/
          '''
        }
      }
    }
  }
}
