podTemplate(
  containers: [
    containerTemplate(
      name: 'kaniko',
      image: 'gcr.io/kaniko-project/executor:latest',
      command: '',
      args: '',
      ttyEnabled: false
    )
  ]
) {
  node(POD_LABEL) {
    container('kaniko') {
      sh '''#!/busybox/sh
      /kaniko/executor \
        --dockerfile=Dockerfile \
        --context=git://github.com/Abhiabhi1019/my-node-app.git \
        --destination=localhost:5000/my-node-app:latest \
        --insecure \
        --skip-tls-verify
      '''
    }
  }
}
