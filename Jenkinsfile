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
    secretVolume(mountPath: '/kaniko/.docker', secretName: 'my-regcred')
  ]
) {
  node(POD_LABEL) {
    stage('Clone Repo') {
      git url: 'https://github.com/Abhiabhi1019/my-node-app.git'
    }

    stage('Build Image with Kaniko') {
      container('kaniko') {
        sh '''
          /kaniko/executor \
            --context `pwd` \
            --dockerfile `pwd`/Dockerfile \
            --destination=docker.io/YOUR_DOCKER_USERNAME/my-node-app:latest \
            --insecure \
            --skip-tls-verify
        '''
      }
    }
  }
}
