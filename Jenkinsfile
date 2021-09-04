pipeline {
  agent {
    kubernetes {
      yaml '''
        apiVersion: v1
        kind: Pod
        metadata:
          labels:
            ctr: node
        spec:
          containers:
          - name: node
            image: node:lts-alpine
            imagePullPolicy: IfNotPresent
            tty: true
            volumeMounts:
            - name: docker-cli
              mountPath: /usr/bin/docker
          volumes:
          - name: docker-cli
            hostPath:
              path: /usr/bin/docker
        '''
    }
  }
  
  stages {
    stage('Build') {
      steps {
        container('node') {
          sh 'yarn install'
        }
      }
    }
    stage('Test') {
      steps {
        container('node') {
          sh 'yarn test'
        }
      }
    }
    stage('Build Image') {
      steps {
        container('node') {
          sh 'docker build -t phucnv282/jenkins-nodejs:latest .'
        }
      }
    }
  }
}
