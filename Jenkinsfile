pipleline {
  agent any
  tools {
    maven 'MAven'
  }
  stages{
    stage('Initialise') {
      steps {
        sh ...
            echo "PATH = $(PATH)"
            echo "M2_HOME = $(M2_HOME)"
          ...
      }
   }
    
    stage ('Build'){
      sh 'mvn clean package'
    }
  }
}
