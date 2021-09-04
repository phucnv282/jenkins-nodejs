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
          - name: docker
            image: docker:latest
            imagePullPolicy: IfNotPresent
            tty: true
            volumeMounts:
            - name: dockersock
              mountPath: /var/run/docker.sock
          volumes:
          - name: dockersock
            hostPath:
              path: /var/run/docker.sock
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
        container('docker') {
          sh 'docker build -t phucnv282/jenkins-nodejs:latest .'
        }
      }
    }
    stage('Push Image') {
      environment {
        DOCKER_HUB_CREDS = credentials('hub.docker.up')
      }
      steps {
        container('docker') {
          def registry_url = 'registry.hub.docker.com/'
          sh 'docker login -u $SDOCKER_HUB_CREDS_USR -p $DOCKER_HUB_CREDS_PSW ${registry_url}'
          sh 'docker push phucnv282/jenkins-nodejs:latest'
          sh 'docker logout'
        }
      }
    }
  }
}
