podTemplate(
  label: 'kaniko-agent',
  containers: [
    containerTemplate(
      name: 'kaniko',
      image: 'gcr.io/kaniko-project/executor:latest',
      command: '/busybox/cat',       // Required dummy command for container to stay alive
      ttyEnabled: true
    )
  ],
  imagePullSecrets: ['regcred'],      // Pull image using DockerHub credentials
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
          url: 'https://github.com/Abhiachu@1019/your-repo.git', // Check: this might be malformed
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
