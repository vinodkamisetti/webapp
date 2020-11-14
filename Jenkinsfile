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
        sh 'sudo rm trufflehog || true'
        sh 'sudo docker run gesellix/trufflehog --json https://github.com/cehkunal/webapp.git > trufflehog'
        sh 'sudo cat trufflehog'
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
  }
}
