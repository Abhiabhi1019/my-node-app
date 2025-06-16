podTemplate(
  containers: [
    containerTemplate(
      name: 'kaniko',
      image: 'gcr.io/kaniko-project/executor:v1.22.0',
      ttyEnabled: true,
      command: '' // ✅ Empty to use Kaniko default entrypoint
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
          --destination=registry.container-registry.svc.cluster.local:5000/my-node-app:latest \
          --insecure \
          --skip-tls-verify
      '''
    }
  }
}
