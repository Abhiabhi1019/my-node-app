podTemplate(
  containers: [
    containerTemplate(
      name: 'kaniko',
      image: 'gcr.io/kaniko-project/executor:latest',
      command: '/kaniko/executor',
      args: '''--context=dir:///workspace/my-node-app \
                --dockerfile=Dockerfile \
                --destination=localhost:5000/my-node-app:latest \
                --insecure \
                --skip-tls-verify''',
      ttyEnabled: false
    )
  ],
  volumes: [
    emptyDirVolume(mountPath: '/workspace', memory: false)
  ]
) {
  node(POD_LABEL) {
    stage('Clone Repo') {
      // Make sure your app gets cloned into /workspace/my-node-app
      dir('my-node-app') {
        git 'https://github.com/your-username/your-node-app.git'
      }
    }

    stage('Build Image') {
      container('kaniko') {
        echo 'Kaniko is building the image...'
        // Kaniko runs automatically using args in the containerTemplate, nothing needed here
      }
    }
  }
}
