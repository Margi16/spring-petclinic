pipeline {
    agent any

    tools {
        // Assumes you have these tools configured in Jenkins Global Tool Configuration
        maven 'Maven 3.6.3' // Replace 'Maven 3.6.3' with your Maven identifier
        jdk 'OpenJDK 11'    // Replace 'OpenJDK 11' with your JDK identifier
    }

    environment {
        // Define environment variables if needed
        SONARQUBE_SCANNER_PARAMS = '-Dsonar.projectKey=spring-petclinic -Dsonar.sources=src -Dsonar.java.binaries=target -Dsonar.host.url=http://yourSonarQubeServer:9000 -Dsonar.login=squ_3b5a85fa4431d9e0ecffca7b877e1a6b0985b9a1'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm // Checks out the source code from SCM
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package' // Build the project and create a package
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // Adjust the SonarQube scanner parameters as needed for your setup
                sh "mvn sonar:sonar ${env.SONARQUBE_SCANNER_PARAMS}"
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test' // Run unit tests
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml' // Publish JUnit test results
                }
            }
        }

        // Add more stages for deployment, Docker build/push, etc., as needed
    }

    post {
        // Post-build actions such as cleanup, notifications, etc.
        always {
            echo 'One or more stages executed...'
        }
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed :('
        }
    }
}
