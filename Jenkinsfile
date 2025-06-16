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
        - /kaniko/executor
      args:
        - "--dockerfile=Dockerfile"
        - "--context=dir:///workspace"
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
