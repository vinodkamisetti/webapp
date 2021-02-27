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
    stage ('Source Composition Analysis') {
      steps {
         sh 'rm owasp* || true'
         sh 'wget "https://raw.githubusercontent.com/cehkunal/webapp/master/owasp-dependency-check.sh" '
         sh 'chmod +x owasp-dependency-check.sh'
         sh 'bash owasp-dependency-check.sh'
         sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
        
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
                sh 'scp -o StrictHostKeyChecking=no target/*.war skamal@52.152.217.58:/home/skamal/tomcat/webapps/webapp.war'
              }      
           }       
    }
    stage ('DAST') {
      steps {
        sshagent(['Tomcat']) {
         sh 'ssh -o  StrictHostKeyChecking=no skamal@52.142.11.68 "docker run -t owasp/zap2docker-stable zap-baseline.py -t http://52.152.217.58:8080/webapp/" || true'
        }
      }
    }
  }
}
