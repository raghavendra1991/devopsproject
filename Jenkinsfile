pipeline {
  agent { docker { image 'python:3.7.2' } }
  environment {
      http_proxy = 'http://127.0.0.1:3128/'
      https_proxy = 'http://127.0.0.1:3128/
      ftp_proxy = 'http://127.0.0.1:3128/'
      socks_proxy = 'socks://127.0.0.1:3128/'
  }
  stages {
    stage('build') {
      steps {
        sh 'pip install requirements.txt'
      }
    }
    stage('test') {
      steps {
        sh 'python3 test.py'
      }   
    }
  }
}
