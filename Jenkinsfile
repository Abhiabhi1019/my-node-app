podTemplate(
  containers: [
    containerTemplate(
      name: 'kaniko',
      image: 'gcr.io/kaniko-project/executor:latest',
      command: '/kaniko/executor',
      args: '--dockerfile=/workspace/my-node-app/Dockerfile --context=dir:///workspace/my-node-app --destination=docker.io/abhiabhi07/my-node-app:latest --insecure --skip-tls-verify'
    )
  ],
  volumes: [
    secretVolume(secretName: 'regcred', mountPath: '/kaniko/.docker')
  ]
) {
  node(POD_LABEL) {
    container('kaniko') {
      echo "✅ Kaniko image build initiated"
    }
  }
}
