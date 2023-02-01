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

    stage('Quality Assurance') {
      parallel {
        stage('Deploy') {
          steps {
            sh './mvnw spring-boot:run </dev/null &>/dev/null &'
          }
        }

        stage('Integration and Performance Test') {
          steps {
            sh './mvnw verify'
            perfReport '  **/target/jmeter/results/*.csv'
          }
        }

      }
    }

    stage('Status Update') {
      steps {
        emailext(subject: 'Build Status', to: 'yogeshpoojari@live.in', body: 'Build Run. Please review the Logs', from: 'CICDBuild ')
      }
    }

    stage('Generate SBOM') {
      steps {
        sh '''curl -sSfL https://raw.githubusercontent.com/anchore/syft/main/install.sh | sh -s -- -b /usr/local/bin
syft app:${BUILD_NUMBER} --scope all-layers -o json > sbom-${BUILD_NUMBER}.json
syft app:${BUILD_NUMBER} --scope all-layers -o table > sbom-${BUILD_NUMBER}.txt'''
        archiveArtifacts(allowEmptyArchive: true, artifacts: 'sbom*', fingerprint: true, onlyIfSuccessful: true)
        sh ' rm -rf sbom*'
      }
    }

  }
}