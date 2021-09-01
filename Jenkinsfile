#!/usr/bin/env groovy
pipeline {
  agent any
  tools {nodejs "lts"}
  stages {
    stage('build') {
      steps {
        sh 'npm install'
      }
    }
    stage('test') {
      steps {
        sh 'npm test'
      }
    }
    stage('run') {
      steps {
        sh 'npm start &'
      }
    }
  }
}
