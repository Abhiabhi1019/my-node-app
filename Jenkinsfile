podTemplate(
  containers: [
    containerTemplate(
      name: 'kaniko',
      image: 'gcr.io/kaniko-project/executor:latest',
      ttyEnabled: true,
      command: '' // ✅ Must be empty so Kaniko uses default ENTRYPOINT
    )
  ],
  volumes: [
    secretVolume(secretName: 'regcred', mountPath: '/kaniko/.docker')
  ]
) {
  node(POD_LABEL) {
    container('kaniko') {
      sh '''
        /kaniko/executor \
          --context `pwd` \
          --dockerfile `pwd`/Dockerfile \
          --destination=localhost:32000/my-node-app:latest \
          --insecure \
          --skip-tls-verify
      '''
    }
  }
}
