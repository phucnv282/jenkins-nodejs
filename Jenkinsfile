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
    container('node') {
      stage('Build') {
        steps {
          sh 'yarn install'
        }
      }
      stage('Test') {
        steps {
          sh 'yarn test'
        }
      }
      stage('Build Image') {
        steps {
          sh 'docker build -t phucnv282/jenkins-nodejs:latest .'
        }
      }
    }
  }
}
