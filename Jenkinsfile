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
      command:
        - ""
      args:
        - "--dockerfile=Dockerfile"
        - "--context=dir:///workspace"
        - "--destination=registry.kube-system.svc.cluster.local:5000/my-node-app:\${BUILD_NUMBER}"
        - "--insecure"
        - "--insecure-push"
      volumeMounts:
        - name: kaniko-secret
          mountPath: /kaniko/.docker
        - name: workspace-volume
          mountPath: /workspace
  volumes:
    - name: kaniko-secret
      secret:
        secretName: regcred
    - name: workspace-volume
      hostPath:
        path: /home/abhi/my-node-app
        type: Directory
  restartPolicy: Never
"""
    }
  }

  environment {
    APP_NAME = "my-node-app"
    IMAGE_TAG = "registry.kube-system.svc.cluster.local:5000/my-node-app:${BUILD_NUMBER}"
    ARGOCD_APP = "my-node-app"
    ARGOCD_SERVER = "argocd-server.argocd.svc.cluster.local"
    ARGOCD_AUTH_TOKEN = credentials('argocd-token-secret-id') // Jenkins secret ID
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
          echo "Kaniko building image ${IMAGE_TAG}"
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
