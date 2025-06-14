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
  ],
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
          url: 'https://github.com/Abhiachu@1019/your-repo.git',
          credentialsId: 'a49b6730-4f6f-4778-9345-e6f1119436cd'  // Replace with your Jenkins GitHub credential ID
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
