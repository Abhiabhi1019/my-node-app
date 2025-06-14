podTemplate(
  label: 'kaniko-agent',
  containers: [
    containerTemplate(
      name: 'kaniko',
      image: 'gcr.io/kaniko-project/executor:latest',
      command: '/busybox/sh',
      args: '-c cat',                 // ✅ Fix here
      ttyEnabled: true
    )
  ],
  imagePullSecrets: ['regcred']
) {
  node('kaniko-agent') {
    stage('Checkout') {
      checkout scm
    }

    stage('Build & Push') {
      container('kaniko') {
        sh '''
          /kaniko/executor \
            --context=dir://$(pwd) \
            --dockerfile=Dockerfile \
            --destination=localhost:5000/my-node-app:latest \
            --insecure \
            --skip-tls-verify
        '''
      }
    }
  }
}

