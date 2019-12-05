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
