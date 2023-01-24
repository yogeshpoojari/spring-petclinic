pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh './mvnw clean compile'
      }
    }

    stage('SonarScan') {
      steps {
        sh '''./mvnw clean sonar:sonar \\
  -Dsonar.projectKey=spring-petclinic \\
  -Dsonar.host.url=http://65.2.36.80:9000 \\
  -Dsonar.login=sqp_6f4fea28fd0538817587d186a387bda667d69082'''
      }
    }

  }
}