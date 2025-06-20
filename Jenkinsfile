pipeline {
  agent {
    kubernetes {
      yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: kaniko
    image: gcr.io/kaniko-project/executor:latest
    command:
    - /kaniko/executor
    args:
    - "--dockerfile=/workspace/Dockerfile"
    - "--context=/workspace/"
    - "--destination=docker.io/abhiabhi007/test-image:latest"
    volumeMounts:
      - name: kaniko-secret
        mountPath: /kaniko/.docker
  volumes:
  - name: kaniko-secret
    secret:
      secretName: dockerhub-secret
"""
    }
  }
  stages {
    stage('Build and Push Docker Image') {
      steps {
        container('kaniko') {
          sh 'echo "Image build and push started"'
        }
      }
    }
  }
}
