podTemplate(
  containers: [
    containerTemplate(
      name: 'kaniko',
      image: 'gcr.io/kaniko-project/executor:latest',
      command: '/busybox/sh',
      args: '-c while true; do sleep 30; done',
      ttyEnabled: true
    )
  ]
) {
  node(POD_LABEL) {
    stage('Build with Kaniko') {
      container('kaniko') {
        sh '''
          /kaniko/executor \
            --context `pwd` \
            --dockerfile `pwd`/Dockerfile \
            --destination docker.io/abhiabhi007/nodejs-app:latest \
            --insecure \
            --skip-tls-verify
        '''
      }
    }
  }
}
