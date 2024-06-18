pipeline {
  agent {
    kubernetes {
      yaml """
apiVersion: v1
kind: Pod
spec:
  nodeSelector:
    kubernetes.io/arch: "amd64"
  containers:
  - name: wolfi
    image: cgr.dev/chainguard/wolfi-base
    command:
    - cat
    tty: true
    resources:
      requests:
        memory: "128Mi"
        cpu: "100m"
      limits:
        memory: "128Mi"
    securityContext:
      allowPrivilegeEscalation: false
      capabilities:
        drop:
        - ALL
      runAsNonRoot: true
      runAsUser: 1000
      seccompProfile:
        type: RuntimeDefault
"""
      retries 2
    }
  }
  stages {
    stage("pre-build") {
      steps {
        container("wolfi") {
            sh "cat /proc/version"
        }
      }
    }
  }
}
