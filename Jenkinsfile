podTemplate(
  containers: [
    containerTemplate(
      name: 'kaniko',
      image: 'gcr.io/kaniko-project/executor:latest',
      command: '',
      args: '',
      ttyEnabled: true
    )
  ],
  volumes: [
    secretVolume(mountPath: '/kaniko/.docker', secretName: 'regcred')
  ]
) {
  node(POD_LABEL) {
    stage('Build Docker Image with Kaniko') {
      container('kaniko') {
        sh '''
        /kaniko/executor \
          --context `pwd` \
          --dockerfile `pwd`/Dockerfile \
          --destination=docker.io/abhiabhi007/nodejs-app:latest \
          --skip-tls-verify
        '''
      }
    }
  }
}

