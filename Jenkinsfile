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
        echo "Listing workspace:"
        ls -la /workspace
        echo "Building Docker image"
        /kaniko/executor \
          --context=dir:///workspace/my-node-app \
          --dockerfile=Dockerfile \
          --destination=localhost:5000/my-node-app:latest \
          --insecure \
          --skip-tls-verify
      '''
    }
  }
}
