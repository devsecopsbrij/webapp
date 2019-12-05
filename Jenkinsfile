pipeline {
  agent any
  tools {
    maven 'maven'
  }
  stages{
    stage ('Initialise') {
      steps {
        sh '''
            echo "PATH = $(PATH)"
            echo "M2_HOME = $(M2_HOME)"
          '''
      }
   }
	  stage ('Check-Git-Keys') {
		  steps {
		   sh 'rm trufflehog || true'	  
		    sh 'docker run gesellix/trufflehog --json https://github.com/devsecopsbrij/webapp.git > trufflehog'	
		    sh 'cat trufflehog'
		  }
	  }
	  stage ('Souce Composistion Analysis'){
		  steps {
			  sh 'rm owasp* || true'
			  sh 'wget "https://raw.githubusercontent.com/devsecopsbrij/webapp/master/owasp-dependency-check.sh"'
			  sh 'chmod +x owasp-dependency-check.sh'
			  sh 'bash owasp-dependency-check.sh'
		  }
	  }
    stage ('Build'){
	steps {
		sh 'mvn clean package'
	}
    }
    stage ('Deploy-To-Tomcat'){
	steps {
		sshagent(['tomcat']){
			sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@3.136.11.24:/prod/apache-tomcat-8.5.49/webapps/webapp.war'
		}
	}
    }
  }
}
