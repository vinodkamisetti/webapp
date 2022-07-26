pipeline {
  agent any 
  tools {
    maven 'Maven'
  }
  stages {
    stage ("Initialize") {
      steps {
          sh'''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
            '''
      }
    }
//    stage ('Trufflehog-Secret-Check') {
//      steps {
//          sh 'rm trufflehog_output.txt || true'
//          sh 'docker run gesellix/trufflehog --json https://github.com/siddkhewal007/webapp.git > trufflehog_output.txt'
//          sh 'cat trufflehog_output.txt'
//    }
//    }
 //   stage ('Sonar-Qube') {
 //     steps {
 //       withSonarQubeEnv('Sonar') {
 //         sh 'mvn sonar:sonar'
 //     }
 //     }
 //   }
    stage ('Build') {
      steps {
      sh 'mvn clean package'
       }
    }
    stage ('Deploy-To-Tomcat') {
            steps {
           sshagent(['tomcat']) {
                sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@18.194.172.249:/opt/tomcat/apache-tomcat-9.0.65/webapps/webapp.war'
              }      
           }       
    }
    stage ('DAST') {
      steps {
        sshagent(['tomcat']) {
         sh 'ssh -o  StrictHostKeyChecking=no ubuntu@54.93.231.51 "docker run -t owasp/zap2docker-stable zap-baseline.py -t http://54.93.231.51:8080/webapp/" || true'
        }
      }
    }
  }
}
//    stage ('Trufflehog') {
//      steps {
//          sh 'rm trufflehog || true'
//          sh 'docker run gesellix/trufflehog --json https://github.com/siddkhewal007/webapp.git >trufflehog'
//          sh 'cat trufflehog'
//      }
//    }
//    stage ('Sonar-Qube') {
//      steps {
//        withSonarQubeEnv('Sonar') {
//          sh 'mvn sonar:sonar'
//      }
//      }
//    }
//     stage ('SCA') {
//       steps {
//          sh 'chmod +x owasp-dependency-check.sh'
//          sh 'sh owasp-dependency-check.sh'  
//      }
//    }
//    stage ('DAST') {
//      steps {
//        sshagent(['Tomcat']) {
//         sh 'ssh -o  StrictHostKeyChecking=no ubuntu@54.93.231.51 "docker run -t owasp/zap2docker-stable zap-baseline.py -t http://54.93.231.51:8080/webapp/" || true'
//        }
//      }
//    }
//    stage ('Source Composition Analysis') {
//      steps {
//         sh 'rm owasp* || true'
//         sh 'wget "https://raw.githubusercontent.com/cehkunal/webapp/master/owasp-dependency-check.sh" '
//         sh 'chmod +x owasp-dependency-check.sh'
//         sh 'bash owasp-dependency-check.sh'
//         sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
//        
//      }
//    }
//   stages {
//     stage ('Initialize') {
//       steps {
//         sh '''
//                     echo "PATH = ${PATH}"
//                     echo "M2_HOME = ${M2_HOME}"
//             ''' 
//       }
//     }
//     stage ('Source Composition Analysis') {
//       steps {
//          sh 'rm owasp* || true'
//          sh 'wget "https://raw.githubusercontent.com/cehkunal/webapp/master/owasp-dependency-check.sh" '
//          sh 'chmod +x owasp-dependency-check.sh'
//          sh 'bash owasp-dependency-check.sh'
//          sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
        
//       }
//     }
//     stage ('Build') {
//       steps {
//       sh 'mvn clean package'
//        }
//     }
//     stage ('Deploy-To-Tomcat') {
//             steps {
//            sshagent(['Tomcat']) {
//                 sh 'scp -o StrictHostKeyChecking=no target/*.war skamal@52.152.217.58:/home/skamal/tomcat/webapps/webapp.war'
//               }      
//            }       
//     }
//     stage ('DAST') {
//       steps {
//         sshagent(['Tomcat']) {
//          sh 'ssh -o  StrictHostKeyChecking=no skamal@52.142.11.68 "docker run -t owasp/zap2docker-stable zap-baseline.py -t http://52.152.217.58:8080/webapp/" || true'
//         }
//       }
//     }
//   }
// }
