podTemplate(
  containers: [
    containerTemplate(
      name: 'kaniko',
      image: 'gcr.io/kaniko-project/executor:latest',
      ttyEnabled: true,
      command: '',
      args: ''
    )
  ]
) {
  node(POD_LABEL) {
    container('kaniko') {
      sh '''
        /kaniko/executor \
          --context `pwd` \
          --dockerfile `pwd`/Dockerfile \
          --destination=docker.io/abhiabhi007/test-image:latest \
          --verbosity=info
      '''
    }
  }
}
