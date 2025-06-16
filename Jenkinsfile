yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    jenkins: kaniko
spec:
  containers:
    - name: kaniko
      image: gcr.io/kaniko-project/executor:latest
      command:
        - /busybox/sh
      args:
        - -c
        - "while true; do sleep 30; done"
      volumeMounts:
        - name: kaniko-secret
          mountPath: /kaniko/.docker
        - name: workspace-volume
          mountPath: /workspace
  volumes:
    - name: kaniko-secret
      secret:
        secretName: regcred
    - name: workspace-volume
      emptyDir: {}
  restartPolicy: Never
"""
