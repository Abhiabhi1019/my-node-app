podTemplate(
    containers: [
        containerTemplate(
            name: 'kaniko',
            image: 'gcr.io/kaniko-project/executor:latest',
            command: '',
            args: '--context=dir:///workspace/my-node-app --dockerfile=/workspace/my-node-app/Dockerfile --destination=docker.io/abhiabhi07/my-node-app:latest --insecure --skip-tls-verify'
        )
    ],
    volumes: [
        secretVolume(secretName: 'regcred', mountPath: '/kaniko/.docker')
    ]
) {
    node(POD_LABEL) {
        container('kaniko') {
            echo "Running Kaniko build..."
            // No need to run anything here. Kaniko runs directly via args.
        }
    }
}
