pipeline {
  agent { docker { image 'python:latest' } }
  
  stages {
    stage('build') {
      environment {
          http_proxy = 'http://127.0.0.1:3128/'
          https_proxy = 'http://127.0.0.1:3128/'
          ftp_proxy = 'http://127.0.0.1:3128/'
          socks_proxy = 'socks://127.0.0.1:3128/'
      }
      steps {
        sh 'pip install -r requirements.txt'
      }
    }
    stage('test') {
      steps {
        sh 'python3 test.py'
      }   
    }
  }
}
