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
