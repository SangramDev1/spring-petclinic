pipeline {
    agent any

    environment {
        M2_HOME = "/opt/apache-maven-3.9.8"  // Use the Maven installation path on the node
        PATH = "${M2_HOME}/bin:${env.PATH}"   // Add Maven to the system PATH
    }

    stages {
        stage('Checkout SCM') {
            steps {
                // Checkout the source code from the Git repository
                checkout scm
            }
        }

        stage('Build & SonarQube Analysis') {
            steps {
                script {
                    // Run Maven build and SonarQube analysis
                    withSonarQubeEnv('SonarQube') { // Ensure 'SonarQube' is configured in Jenkins
                        sh "mvn clean verify sonar:sonar"
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    script {
                        def qg = waitForQualityGate()
                        if (qg.status != 'OK') {
                            error "Pipeline aborted due to quality gate failure: ${qg.status}"
                        }
                    }
                }
            }
        }

        stage('Archiving the Artifacts and Test Results') {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true
                junit '**/target/surefire-reports/*.xml'
            }
        }
    }

    post {
        always {
            // Clean up workspace after build
            cleanWs()
        }
        success {
            echo 'Build and SonarQube Analysis successful!'
        }
        failure {
            echo 'Build failed.'
        }
    }
}
