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
    - cat
    tty: true
    volumeMounts:
    - name: kaniko-secret
      mountPath: /kaniko/.docker
  volumes:
  - name: kaniko-secret
    projected:
      sources:
      - secret:
          name: regcred
"""
        }
    }
    stages {
        stage('Build and Push') {
            steps {
                container('kaniko') {
                    sh '''/kaniko/executor \
  --context `pwd` \
  --dockerfile `pwd`/Dockerfile \
  --destination=registry.local:5000/nodejs-app:v2 \
  --insecure \
  --skip-tls-verify'''
                }
            }
        }
    }
}
