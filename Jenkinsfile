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
          sh './mvnw clean verify sonar:sonar -Dsonar.projectKey=sq1 -Dsonar.host.url=http://sonarqube:9000 -Dsonar.login=squ_4cbf4fe8aa242daceebf99fbaebafecdcea170f7'
        }
      }
    }

    stage('Deploy') {
      steps {
        sshagent(['sshKey']) {
          sh '/usr/bin/ansible-playbook deploy.yml -i inventory.ini'
        }
      }
    }
  }
  post {
        success {
            echo "Build succeeded!"
        }
        failure {
            echo "Build failed.."
        }
        always {
            echo "Pipeline completed!"
        }
    }

}