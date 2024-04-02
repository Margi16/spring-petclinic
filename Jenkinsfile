pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        git(url: 'https://github.com/Margi16/spring-petclinic.git', branch: 'main')
        sh './mvnw clean package'
      }
    }
    stage('Sonarqube') {
      steps {
        sh './mvnw clean verify sonar:sonar -Dsonar.projectKey=jenkins -Dsonar.host.url=http://localhost:9000 -Dsonar.login=squ_3b5a85fa4431d9e0ecffca7b877e1a6b0985b9a1'
      }
    }
    
    stage('Run') {
      steps {
        sh 'java -jar target/spring-petclinic-3.0.0-SNAPSHOT.jar --server.port=8081 &'
      }
    }
  }
}