pipeline {
    agent any

    environment {
        // Specify Maven home path. Modify the path as per your Jenkins environment setup.
        M2_HOME = '/usr/share/maven' // Example path, adjust based on your setup
        PATH = "${M2_HOME}/bin:${env.PATH}"
    }

    stages {
        stage('Checkout Source') {
            steps {
                git url: 'https://github.com/SangramDev1/spring-petclinic.git', branch: 'main'
            }
        }

        stage('Build & SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv('SONAR_LATEST') { // Ensure 'SONAR_LATEST' is your configured SonarQube server name in Jenkins
                        sh "${M2_HOME}/bin/mvn clean compile sonar:sonar"
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Build and SonarQube Analysis completed successfully!'
        }
        failure {
            echo 'Build failed. Check logs for errors.'
        }
        always {
            cleanWs()
        }
    }
}
