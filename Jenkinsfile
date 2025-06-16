podTemplate(
  name: 'kaniko',
  yaml: """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: kaniko
    image: gcr.io/kaniko-project/executor:latest
    command:
    - /busybox/cat
    tty: true
    volumeMounts:
    - name: kaniko-secret
      mountPath: /kaniko/.docker
  volumes:
  - name: kaniko-secret
    secret:
      secretName: regcred
"""
) {
  node('kaniko') {
    container('kaniko') {
      sh '''#!/busybox/sh
        /kaniko/executor \
          --context `pwd` \
          --dockerfile `pwd`/Dockerfile \
          --destination registry.container-registry.svc.cluster.local:5000/test:latest \
          --insecure \
          --skip-tls-verify
      '''
    }
  }
}
