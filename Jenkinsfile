podTemplate(
  containers: [
    containerTemplate(
      name: 'kaniko',
      image: 'gcr.io/kaniko-project/executor:latest',
      command: '',
      args: ''
    )
  ],
  volumes: [
    secretVolume(secretName: 'dockerhub-secret', mountPath: '/kaniko/.docker')
  ]
) {
  node(POD_LABEL) {
    container('kaniko') {
      sh '''
        /kaniko/executor \
          --dockerfile=Dockerfile \
          --context=. \
          --destination=docker.io/abhiabhi007/test-image:latest \
          --verbosity=info
      '''
    }
  }
}
