pipeline {
  agent { label 'java' }

  options {
    timeout(time: 1, unit: 'HOURS')
    retry(2)
  }

  triggers {
    cron('0 * * * *') // Runs every day at midnight
  }

  stages {
    stage('Source Code Pulling') {
      steps {
        git branch: 'main', url: 'https://github.com/SangramDev1/spring-petclinic.git'
      }
    }

    stage('Build & SonarQube Analysis') {
      steps {
        withSonarQubeEnv('SONAR_LATEST') { // Make sure SonarQube server is configured
          withEnv(["M2_HOME=/opt/apache-maven-LATEST", "PATH=$M2_HOME/bin:$PATH"]) { // Update to latest Maven version
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
