podTemplate(
  containers: [
    containerTemplate(
      name: 'kaniko',
      image: 'gcr.io/kaniko-project/executor:latest',
      command: '/kaniko/executor',
      args: ['--help'],  // Run something safe to start container
      ttyEnabled: true
    )
  ]
) {
  node(POD_LABEL) {
    container('kaniko') {
      // Run the actual Kaniko build command
      sh '''
        /kaniko/executor \
          --context `pwd` \
          --dockerfile `pwd`/Dockerfile \
          --destination docker.io/abhiabhi007/nodejs-app:latest \
          --insecure \
          --skip-tls-verify
      '''
    }
  }
}

