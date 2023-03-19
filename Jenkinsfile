pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub-admin')
  }
  stages {
    stage('Checkout Flask-APP') {
      steps {
        git url: 'https://github.com/AhmadKObeid/flask_app.git', branch : 'main', credentialsId: 'github-admin'
      }
    }
    stage('Build and Test') {
      steps {
          sh 'docker build -t ahmadobeid/flask-app .'
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push') {
      steps {
          sh 'docker push ahmadobeid/flask-app'
      }
    }
  }
  stage('Checkout Flask-APP-OPS') {
      steps {
        git url: 'https://github.com/AhmadKObeid/flask_app_ops.git', branch : 'main', credentialsId: 'github-admin'
      }
    }
  stage('Deploy') {
    steps {
          sh 'ls'
      }
    }
  }
  post {
    always {
        sh 'docker logout'
    }
  }
