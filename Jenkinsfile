podTemplate(
  label: 'kaniko-agent',
  containers: [
    containerTemplate(
      name: 'kaniko',
      image: 'gcr.io/kaniko-project/executor:latest',
      ttyEnabled: true
    )
  ],
  imagePullSecrets: ['regcred']
) {
  node('kaniko-agent') {
    stage('Checkout') {
      checkout scm
    }

    stage('Build & Push Docker Image') {
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
