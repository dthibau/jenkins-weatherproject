pipeline {
  agent any
  tools {
      maven 'Maven3' 
  }
 
 stages { 
    stage('Show env') {
      steps {
        echo "ID Git : ${env.GIT_COMMIT}"
      }
    }
    
    
    
    stage('Tests') {
      steps {
       echo "Test staring"
      }
   }
}
}