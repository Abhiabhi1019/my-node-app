podTemplate(
  label: 'kaniko-agent',
  containers: [
    containerTemplate(
      name: 'kaniko',
      image: 'gcr.io/kaniko-project/executor:latest',
      command: '/busybox/sh',
      args: ['-c', 'cat'],
      ttyEnabled: true
    )
  ],
  imagePullSecrets: ['regcred']
) {
  node('kaniko-agent') {
    stage('Checkout') {
      checkout([$class: 'GitSCM',
        branches: [[name: '*/main']],
        userRemoteConfigs: [[
          url: 'https://github.com/your-user/your-repo.git',
          credentialsId: 'your-creds-id'
        ]]
      ])
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
