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
        memory: "768Mi"
        cpu: "100m"
      limits:
        memory: "768Mi"
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
    stage('readCache') {
      steps {
        readCache name: 'mvn-cache'
      }
    }
    stage('build') {
      steps {
        container("maven") {
          sh 'mvn clean package -Dquarkus.package.jar.type=uber-jar -Dmaven.repo.local=./maven-repo -DskipTests'
        }
      }
      post {
        success {
          archiveArtifacts artifacts: 'target/*-runner.jar', followSymlinks: false
          writeCache name: 'mvn-cache', includes: 'maven-repo/**'
        }
      }
    }
  }
}
