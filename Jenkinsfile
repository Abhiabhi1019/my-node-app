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
        - name: workspace-volume
          mountPath: /workspace
  volumes:
    - name: kaniko-secret
      secret:
        secretName: regcred
    - name: workspace-volume
      emptyDir: {}
  restartPolicy: Never
"""
    }
  }

  stages {
    stage('Build with Kaniko') {
      steps {
        container('kaniko') {
          sh '''
            /kaniko/executor \
              --dockerfile=Dockerfile \
              --context=dir:///workspace \
              --destination=registry.kube-system.svc.cluster.local:5000/my-node-app:${BUILD_NUMBER} \
              --insecure \
              --insecure-push
          '''
        }
      }
    }
  }
}
