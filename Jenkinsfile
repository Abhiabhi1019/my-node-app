podTemplate(
  containers: [
    containerTemplate(
      name: 'kaniko',
      image: 'gcr.io/kaniko-project/executor:v1.22.0',
      command: ["/kaniko/executor"],
      args: [
        "--context=dir:///workspace",
        "--dockerfile=/workspace/Dockerfile",
        "--destination=registry.container-registry.svc.cluster.local:5000/my-node-app:latest",
        "--insecure",
        "--skip-tls-verify"
      ],
      volumeMounts: [
        volumeMount(mountPath: '/workspace', name: 'workspace'),
        volumeMount(mountPath: '/kaniko/.docker', name: 'docker-config')
      ]
    )
  ],
  volumes: [
    hostPathVolume(hostPath: '/home/abhi/my-node-app', mountPath: '/workspace'),
    secretVolume(secretName: 'regcred', mountPath: '/kaniko/.docker')
  ]
) {
  node(POD_LABEL) {
    container('kaniko') {
      echo "Building image..."
    }
  }
}
