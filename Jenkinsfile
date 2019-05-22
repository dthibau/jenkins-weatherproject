@Library("SAMPLE") _

pipeline {
  agent any
  tools {
      maven 'M3' 
  }
 stages {
    stage('Unit') {
      steps {
        unitTest {}
        stash includes: '**/target/*.war', name: 'webapp'
      }
    }
    
    
    stage('Parallel Stage') {
      when {
        not {
          allOf {
            branch 'master'
          }
        }
        beforeAgent true
      }
      parallel {
        stage('Integration tests') {
          agent any
          steps {  
            sh "mvn -Pintegration integration-test"
          }
        }
        stage('Qualité') {
          agent any
          steps {  
            sonar 'sonar-weather.properties'
          }
        }
      }
    }
    stage('Validation métier') {
      agent none     
       input {
          message "Déploiement en prod ?"
          ok "Oui "
          parameters {
            string(name: 'DATA_CENTER', defaultValue: 'Paris', description: 'Data Center où déployer')
          }
        }
      steps {
        echo "Echo, let's go to prod"
      }
    }
    stage('Déploiement') {
      agent {
        label 'windows'
      }
     steps {
        unstash 'webapp'  
        script {
          if ( params.DATA_CENTER == 'Paris') {
            sh 'cp simple-parent/simple-webapp/target/*.war /home/dthibau/Formations/Jenkins/MyWork/Serveur/Paris.war'
          } else {
            sh 'cp simple-parent/simple-webapp/target/*.war /home/dthibau/Formations/Jenkins/MyWork/Serveur/Bruxelles.war'
          }
        }
     }
    } 
 }           
}
 
