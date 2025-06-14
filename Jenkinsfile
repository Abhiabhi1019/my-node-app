pipeline {
  agent {
    kubernetes {
      yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    jenkins: kaniko
spec:
  containers:
    - name: kaniko
      image: gcr.io/kaniko-project/executor:latest
      args:
        - "--dockerfile=Dockerfile"
        - "--context=dir://workspace"
        - "--destination=registry.kube-system.svc.cluster.local:5000/my-node-app:\${BUILD_NUMBER}"
        - "--insecure"
        - "--insecure-push"
      volumeMounts:
        - name: kaniko-secret
          mountPath: /kaniko/.docker
  volumes:
    - name: kaniko-secret
      secret:
        secretName: regcred
  restartPolicy: Never
"""
    }
  }

  environment {
    APP_NAME = "my-node-app"
    IMAGE_TAG = "registry.kube-system.svc.cluster.local:5000/my-node-app:${BUILD_NUMBER}"
    ARGOCD_APP = "my-node-app"
    ARGOCD_SERVER = "argocd-server.argocd.svc.cluster.local"
    ARGOCD_AUTH_TOKEN = credentials('argocd-token-secret-id') // Use Jenkins secret ID here
  }

  stages {
    stage('Clone') {
      steps {
        git url: 'https://github.com/Abhiabhi1019/my-node-app.git', branch: 'main'
      }
    }

    stage('Build & Push with Kaniko') {
      steps {
        container('kaniko') {
          // No additional shell steps needed; args are already set in the pod template
          echo "Kaniko build running with arguments defined in pod YAML."
        }
      }
    }

    stage('Deploy via ArgoCD') {
      steps {
        sh '''
          curl -k -H "Authorization: Bearer $ARGOCD_AUTH_TOKEN" \
            -X POST \
            "$ARGOCD_SERVER/api/v1/applications/$ARGOCD_APP/sync"
        '''
      }
    }
  }
}
