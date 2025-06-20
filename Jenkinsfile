podTemplate(
  containers: [
    containerTemplate(
      name: 'kaniko',
      image: 'gcr.io/kaniko-project/executor:latest',
      command: '/busybox/cat',
      ttyEnabled: true
    )
  ],
  volumes: [
    secretVolume(secretName: 'regcred-new', mountPath: '/kaniko/.docker')
  ]
) {
  node(POD_LABEL) {
    container('kaniko') {
      sh '''
        /kaniko/executor \
          --dockerfile=/workspace/my-node-app/Dockerfile \
          --context=dir:///workspace/my-node-app \
          --destination=docker.io/abhiabhi007/test-image:latest \
          --insecure \
          --skip-tls-verify
      '''
    }
  }
}
