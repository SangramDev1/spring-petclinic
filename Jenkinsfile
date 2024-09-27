pipeline {
  agent { label 'java' }

  options {
    timeout(time: 1, unit: 'HOURS')
    retry(2)
  }

  triggers {
    cron('0 * * * *')
  }

  stages {
    stage('Source Code Pulling') {
      steps {
        git branch: 'main', url: 'https://github.com/SangramDev1/spring-petclinic.git'
      }
    }

    stage('Build & SonarQube Analysis') {
      steps {
        withEnv(["M2_HOME=/opt/apache-maven-3.9.8", "PATH=$M2_HOME/bin:$PATH"]) { // Set M2_HOME here
          withSonarQubeEnv('SONAR_LATEST') { // Make sure SonarQube server is configured
            sh 'mvn clean package sonar:sonar'
          }
        }
      }
    }

    stage('Archiving the Artifacts and Test Results') {
      steps {
        junit '**/target/surefire-reports/*.xml' // Check the exact location of test reports
        // Optional: Archive build artifacts if needed (e.g., archiveArtifacts 'target/*.jar')
      }
    }
  }

  post {
    success {
      echo 'Build succeeded'
    }
    failure {
      echo 'Build failed'
    }
  }
}
