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
            withEnv(["HOME=${env.WORKSPACE}"]) {
                sh "pip install -r requirements.txt --user"
            }
      }
    }
    stage('test') {
      steps {
        sh 'python3 test.py'
      }   
    }
    stage('SonarQube Analysis') {
		    environment {
			       scannerHome = tool 'SonarQube Scanner'
        }
		  steps {
            echo '********* Test Stage Started **********'			
				  withSonarQubeEnv('admin') {
					  sh '${scannerHome}/bin/sonar-scanner \
					  -D sonar.projectKey=pybuilder \
					  -Dsonar.python.coverage.reportPaths=coverage.xml'
				  }
		        echo '********* Test Stage Finished **********'
		   }
	   }
    stage ('Generate Test Reports') {
        steps {
            sh 'pyb publish'       
        }
    }
	  stage ('Publish Artifactory') {
	     steps {
	      	echo '********* Publish Report to JFrog Artifacts **********' 
			    withCredentials([usernamePassword(credentialsId: 'artifactory', passwordVariable: 'passwd', usernameVariable: 'user')]) {
				    sh 'jf rt upload target/ pythonapp/'
			  }
		       echo '********* Publish Report Finished **********'	
	     }
	   }
  }
}
