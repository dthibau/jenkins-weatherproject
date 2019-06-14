pipeline {
  agent any
  tools {
      maven 'Maven3' 
  }
 stages {
    stage('Build') {
      steps {
        sh 'mvn -Dmaven.test.failure.ignore clean package'
      }
    }
    
    
    stage('Results') {
      steps {
        junit '**/target/surefire-reports/TEST-*.xml'
        archiveArtifacts '**/target/*.jar,**/target/*.war'
      }
      
   }

}
 
}