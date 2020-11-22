pipeline {
  agent any 
  tools {
    maven 'Maven'
  }
  stages {
    stage ('Initialize') {
      steps {
        sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
            ''' 
      }
    }
    stage ('Check-Git-Secrets') {
      steps {
        sh 'docker run gesellix/trufflehog --json https://github.com/siddkhewal007/webapp.git > trufflehog'
        sh 'cat trufflehog'
      }
    }
    stage ('Source Composition Analysis') {
      steps {
         sh 'rm owasp* || true'
         sh 'wget "https://raw.githubusercontent.com/cehkunal/webapp/master/owasp-dependency-check.sh" '
         sh 'chmod +x owasp-dependency-check.sh'
         sh 'bash owasp-dependency-check.sh'
         sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
        
      }
    }
    stage ('SAST') {
      steps {
        withSonarQubeEnv('Sonar') {
          sh 'mvn sonar:sonar'
          sh 'cat target/sonar/report-task.txt'
        }
      }
    }
    stage ('Build') {
      steps {
      sh 'mvn clean package'
       }
    }
    stage ('Deploy-To-Tomcat') {
            steps {
           sshagent(['Tomcat']) {
                sh 'scp -o StrictHostKeyChecking=no target/*.war sidd@40.76.5.105:/home/sidd/prod/apache-tomcat-8.5.59/webapps/webapp.war'
              }      
           }
    }
    stage ('DAST') {
      steps {
        sshagent(['ZAP']) {
         sh 'ssh -o  StrictHostKeyChecking=no sidd@40.76.2.234 "docker run -t owasp/zap2docker-stable zap-baseline.py -t http://40.76.5.105:8080/webapp/ -r report.html" || true'
        }
      }
    }
    
  }
}
