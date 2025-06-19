podTemplate(
  containers: [
    containerTemplate(
      name: 'kaniko',
      image: 'gcr.io/kaniko-project/executor:latest',
      command: '/kaniko/executor',
      args: ['--help'], // dummy safe start
      ttyEnabled: true
    )
  ]
) {
  node(POD_LABEL) {
    stage('Kaniko Build') {
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
