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

  }
}