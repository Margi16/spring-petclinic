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
        withSonarQubeEnv('sq1') {
          sh './mvnw clean verify sonar:sonar -Dsonar.projectKey=jenkins -Dsonar.host.url=http://sonarqube:9000 -Dsonar.login=squ_3b5a85fa4431d9e0ecffca7b877e1a6b0985b9a1'
        }
      }
    }
    
    stage('Run') {
      steps {
        sh 'java -jar /var/jenkins_home/workspace/spc_main/target/spring-petclinic-3.2.0-SNAPSHOT.jar -Dserver.port=8081 &'
      }
    }
  }
}