podTemplate(
  containers: [
    containerTemplate(
      name: 'kaniko',
      image: 'gcr.io/kaniko-project/executor:v1.20.0',
      command: '',
      args: '',
      ttyEnabled: true
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
