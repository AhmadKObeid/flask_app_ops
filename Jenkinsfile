pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub-admin')
    DOCKER_OPTS="-H tcp://0.0.0.0:2376 -H unix:///var/run/docker.sock"
  }
  stages {
    stage('Checkout Flask-APP') {
      steps {
        git url: 'https://github.com/AhmadKObeid/flask_app.git', branch : 'main', credentialsId: 'github-admin'
      }
    }
    stage('Build and Test') {
      steps {
        container('docker') {
          sh 'docker build -t ahmadobeid/flask-app .'
        }
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push') {
      steps {
        container('docker') {
          sh 'docker push ahmadobeid/flask-app'
        }
      }
    }
  }
  post {
    always {
      container('docker') {
        sh 'docker logout'
      }
    }
  }
}
