podTemplate(
  label: 'kaniko-agent',
  containers: [
    containerTemplate(
      name: 'kaniko',
      image: 'gcr.io/kaniko-project/executor:latest',
      command: '/kaniko/executor',
      args: ['--help'],  // Keeps the container alive
      ttyEnabled: true
    )
  ],
  imagePullSecrets: ['regcred'],
  volumes: [
    secretVolume(
      secretName: 'regcred',
      mountPath: '/kaniko/.docker'
    )
  ]
) {
  node('kaniko-agent') {
    stage('Checkout Code') {
      checkout([$class: 'GitSCM',
        branches: [[name: '*/main']],
        userRemoteConfigs: [[
          url: 'https://github.com/Abhiachu/your-repo.git',
          credentialsId: 'a49b6730-4f6f-4778-9345-e6f1119436cd'
        ]]
      ])
    }

    stage('Build & Push with Kaniko') {
      container('kaniko') {
        sh '''
        /kaniko/executor \
          --context `pwd` \
          --dockerfile `pwd`/Dockerfile \
          --destination localhost:5000/my-node-app:latest \
          --insecure \
          --skip-tls-verify
        '''
      }
    }
  }
}
