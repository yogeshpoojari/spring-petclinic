pipeline {
  agent any
  stages {
    stage('Clean') {
      steps {
        sh './mvnw clean'
      }
    }

    stage('Compile') {
      steps {
        sh './mvnw compile'
      }
    }

    stage('SonarQube Scan') {
      steps {
        sh '''./mvnw sonar:sonar \\
  -Dsonar.projectKey=spring-petclinic \\
  -Dsonar.host.url=http://172.31.26.125:9000 \\
  -Dsonar.login=sqp_6f4fea28fd0538817587d186a387bda667d69082'''
      }
    }

    stage('Unit Test') {
      steps {
        sh '''chromedriver
./mvnw test
junit \'**//target/surefire-reports/TEST-*.xml\''''
      }
    }

  }
}