podTemplate(
  containers: [
    containerTemplate(
      name: 'kaniko',
      image: 'gcr.io/kaniko-project/executor:latest',
      command: '/busybox/sh',
      args: '-c sleep 9999999',
      ttyEnabled: true
    )
  ]
) {
  node(POD_LABEL) {
    container('kaniko') {
      // Your Kaniko build command
      sh '''
        /kaniko/executor \
        --context `pwd` \
        --dockerfile `pwd`/Dockerfile \
        --destination abhiabhi007/nodejs-app:latest \
        --insecure
      '''
    }
  }
}
