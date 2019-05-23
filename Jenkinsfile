pipeline {
  agent none
  tools {
      maven 'Maven3' 
  }
  triggers {
    pollSCM 'H/5 * * * *'
  }

 
 stages { 
    stage('Show env') {
        agent any
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
                agent any
                steps {  
                    sh 'mvn -Pintegration clean integration-test'  
                }
            }
            stage('Analyse qualité') {
                agent any
                steps {  
                    echo "On Branch B" 
                    sh 'mvn clean verify'
                    script {

                      def scannerHome = tool name: 'SSCAN3', type: 'hudson.plugins.sonar.SonarRunnerInstallation'

                      withSonarQubeEnv('SONAR6') {
                        sh "${scannerHome}/bin/sonar-scanner -Dproject.settings=sonar-weather.properties"
                      }
/*                      timeout(time: 10, unit: 'MINUTES') {
                        waitForQualityGate abortPipeline: true
                      }*/
                    }
                }
                
            }
             
        }
    }
    stage('Validation métier') {
               agent none
               options {
                  timeout(2)
                }
               input {
                  message 'Vers quel data center voulez vous déployer ?'
                  ok 'Déployer'
                  parameters {
                    choice choices: ['Paris', 'New York', 'Abidjan'], description: '', name: 'DataCenter'
                  }
                }
                steps {
                  echo "Data center is ${DataCenter}"
                  script {
                      if ( DataCenter.equals('Paris') ) {
                          echo 'Pas de serveur désolé'
                          currentBuild.result = 'UNSTABLE'
                      } else {
                          echo "C'est déployé"
                      }
                  }
                }
             }
  }
}