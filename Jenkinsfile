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
  - name: maven
    image: maven:3.9.7-eclipse-temurin-17
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
    stage("verification") {
      steps {
        container("maven") {
            sh "mvn --version"
        }
      }
    }
  }
}
