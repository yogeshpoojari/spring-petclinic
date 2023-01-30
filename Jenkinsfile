pipeline {
  agent {
    node {
      label 'test'
    }

  }
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
  -Dsonar.host.url=http://172.31.30.225:9000 \\
  -Dsonar.login=sqp_9353649e7455b02980e5ac18e1a87df2be3faf37'''
      }
    }

    stage('Unit Test') {
      steps {
        sh '''./mvnw "-Dtest=**/petclinic/*/*.java" test
'''
        junit '**/target/surefire-reports/TEST-*.xml'
      }
    }

    stage('Package') {
      steps {
        sh './mvnw package -DskipTests=true'
      }
    }

    stage('Deploy') {
      parallel {
        stage('Deploy') {
          steps {
            sh './mvnw spring-boot:run </dev/null &>/dev/null &'
          }
        }

        stage('Integration and Performance Test') {
          steps {
            sh './mvnw verify -DskipUnitTests'
            perfReport '  **/target/jmeter/results/*.*'
          }
        }

      }
    }

  }
}