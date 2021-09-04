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
        '''
    }
  }
  
  stages {
    stage('Build') {
      container('node') {
        steps {
          sh 'yarn install'
        }
      }
    }
    stage('Test') {
      container('node') {
        steps {
          sh 'yarn test'
        }
      }
    }
    stage('Build Image') {
      container('node') {
        steps {
          sh 'docker build -t phucnv282/jenkins-nodejs:latest .'
        }
      }
    }
  }
}
