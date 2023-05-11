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
    
    stage ('Source Composition Analysis'){
      steps {
        sh 'rm owasp* || true'
        sh 'wget "https://raw.githubusercontent.com/GevDa140/webapp/main/owasp-dp-checker.sh" '
        sh 'chmod +x owasp-dp-checker.sh'
        sh 'bash owasp-dp-checker.sh'
        sh 'cat /var/lib/jenkins/workspace/Webapp-cicd-pipeline/odc-reports/dependency-check-report.xml'
      }
    }
    
    stage ('Build') {
      steps {
        sh 'mvn clean package'
      }
    }
    
    stage ('Deploy-To-Tomcat') {
            steps {
           sshagent(['tomcat']) {
                sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@62.84.123.176:/prod/apache-tomcat-9.0.71/webapps/webapp.war'
               }
            }
    }
    
  }
}
