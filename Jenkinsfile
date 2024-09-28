pipeline {
    agent { label 'java' }

    stages {
        stage('Source Code Pulling') {
            steps {
                git branch: 'main', url: 'https://github.com/SangramDev1/spring-petclinic.git'
            }
        }

        stage('Build') {
            steps {
                sh '/opt/apache-maven-3.9.8/bin/mvn clean package'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SONAR_LATEST') {
                    sh '/opt/apache-maven-3.9.8/bin/mvn sonar:sonar'
                }
            }
        }

        stage('Archiving Artifacts and Test Results') {
            steps {
                junit '**/target/surefire-reports/*.xml'
                archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true
            }
        }
    }
}
