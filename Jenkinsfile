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
                script {
                    // Wrap the environment configuration in a script block
                    withSonarQubeEnv('SONAR_LATEST') { 
                        withEnv(["M2_HOME=/opt/apache-maven-3.9.8", "PATH=$M2_HOME/bin:$PATH"]) {
                            // Execute Maven with increased verbosity to debug issues
                            sh 'mvn -X clean package sonar:sonar'
                        }
                    }
                }
            }
        }

        stage('Archiving the Artifacts and Test Results') {
            steps {
                // Archive test results
                junit '**/target/surefire-reports/*.xml'
            }
        }
    }

    post {
        always {
            // Always clean workspace
            cleanWs()
        }
        success {
            echo 'Build succeeded'
        }
        failure {
            echo 'Build failed'
        }
    }
}
