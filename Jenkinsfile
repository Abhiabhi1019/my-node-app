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
    IMAGE_TAG = "registry.kube-system.svc.cluster.local:5000/my-node-app:${BUILD_NUMBER}"
    ARGOCD_APP = "my-node-app"
    ARGOCD_SERVER = "argocd-server.argocd.svc.cluster.local"
    ARGOCD_AUTH_TOKEN = credentials('eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJhcmdvY2QiLCJzdWIiOiJhZG1pbjphcGlLZXkiLCJuYmYiOjE3NDk4OTY3ODMsImlhdCI6MTc0OTg5Njc4MywianRpIjoiYTk2Y2QyOTQtMDg5OS00MGIxLWIzY2YtZWVhYWIwYzdhZWRjIn0.RhRXhlykd12kQ76GVuWZ807YTOohlSjWcFp44H7jgAQ') // set in Jenkins
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
              --context=`pwd` \
              --destination=$IMAGE_TAG \
              --insecure \
              --insecure-push
          '''
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
