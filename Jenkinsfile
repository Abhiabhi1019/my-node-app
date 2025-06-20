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
        - --dockerfile=/workspace/Dockerfile
        - --context=dir:///workspace
        - --destination=docker.io/abhiabhi007/my-node-app:latest
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
          sh 'echo "Kaniko build should run now..."'
        }
      }
    }
  }
}
