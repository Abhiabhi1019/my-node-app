podTemplate(
  label: 'kaniko-agent',
  containers: [
    containerTemplate(
      name: 'kaniko',
      image: 'gcr.io/kaniko-project/executor:latest',
      command: '',
      args: '',
      ttyEnabled: true
    )
  ]
) {
  node('kaniko-agent') {
    stage('Clone Repo') {
      checkout scm
    }

    stage('Build & Push Image') {
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
