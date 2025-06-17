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
                --skip-tls-verify'''
    )
  ],
  volumes: [
    emptyDirVolume(mountPath: '/workspace', memory: false)
  ]
) {
  node(POD_LABEL) {
    stage('Clone') {
      dir('/workspace/my-node-app') {
        // Replace with your repo
        git 'https://github.com/your-user/your-node-app.git'
      }
    }

    stage('Build') {
      container('kaniko') {
        echo 'Kaniko runs with args at container start'
      }
    }
  }
}
