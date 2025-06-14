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
    - cat
    tty: true
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
    IMAGE_TAG = "registry.kube-system.svc.cluster.local:5000/my-node-app:\${BUILD_NUMBER}"
    ARGOCD_APP = "my-node-app"
    ARGOCD_SERVER = "argocd-server.argocd.svc.cluster.local"
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
          sh '''
            /kaniko/executor \
              --dockerfile=Dockerfile \
              --context=. \
              --destination=$IMAGE_TAG \
              --insecure \
              --insecure-push
          '''
        }
      }
    }

    stage('Deploy via ArgoCD') {
      environment {
        ARGOCD_AUTH_TOKEN = credentials('ARGOCD_AUTH_TOKEN') // ID from Jenkins secret
      }
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
