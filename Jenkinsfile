pipeline {
  agent any
  tools {
      maven 'Maven3' 
  }
 
 stages { 
    stage('Show env') {
      steps {
        echo "ID Git : ${env.GIT_COMMIT}"
        sh "mvn clean package"
      } 
      post {
          always {
              junit '**/target/surefire-reports/*.xml'
          } 
          success {
              archive '**/target/*.war'
          }
          failure {
              mail bcc: '', body: 'Planté !!', cc: '', from: '', replyTo: '', subject: '', to: 'david.thibau@gmail.com'
          }
      } 
      
    }
    
    
    
    stage('Phases parallèles') {
        parallel {
            stage("Tests d'intégration") {
                steps {  
                    sh 'mvn -Pintegration integration-test'  
                }
            }
            stage('Analyse qualité') {
                steps {  
                    echo "On Branch B" 
                    sh 'mvn clean verify'
                    script {

                      def scannerHome = tool 'SSCAN3'

                      withSonarQubeEnv('SONAR6') {
                        sh "${scannerHome}/bin/sonar-scanner -Dproject.settings=sonar-weather.properties"
                      }
                      timeout(time: 10, unit: 'MINUTES') {
                        waitForQualityGate abortPipeline: true
                      }
                    }
                }
                
            }
        }
    }
}
}