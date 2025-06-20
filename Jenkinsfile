podTemplate(
  containers: [
    containerTemplate(
      name: 'kaniko',
      image: 'gcr.io/kaniko-project/executor:latest',
      command: '',
      args: '''/kaniko/executor
        --context=dir:///workspace/my-node-app
        --dockerfile=/workspace/my-node-app/Dockerfile
        --destination=docker.io/abhiabhi07/my-node-app:latest
        --insecure
        --skip-tls-verify
      '''
    )
  ],
  volumes: [
    secretVolume(secretName: 'regcred', mountPath: '/kaniko/.docker')
  ]
) {
  node(POD_LABEL) {
    container('kaniko') {
      // Kaniko will run automatically via args; nothing more needed here
    }
  }
}
