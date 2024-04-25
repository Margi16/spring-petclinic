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
    
    // not needed - comment
    // stage('Run') {
    //   steps {
    //     sh 'java -jar /var/jenkins_home/workspace/spring-petclinic_main/target/spring-petclinic-3.2.0-SNAPSHOT.jar -Dserver.port=8081 &'
    //   }
    // }

    stage('Deploy') {
      steps {
      // Ensure the inventory file points to your production server
      sshagent(['sshKey']) {
        sh 'ansible-playbook ansible/deploy.yml -i ansible/inventory.ini'
        //     ansiblePlaybook(playbook: 'deploy.yml', inventory: 'inventory.ini')
      }
      }
    }
  }
}