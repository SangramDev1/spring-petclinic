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
                withSonarQubeEnv('SONAR_LATEST') { 
                    withEnv(["M2_HOME=/opt/apache-maven-3.9.8", "PATH=$M2_HOME/bin:$PATH"]) {
                        sh 'mvn clean package sonar:sonar'
                    }
                }
            }
        }

        stage('Archiving the Artifacts and Test Results') {
            steps {
                junit '**/target/surefire-reports/*.xml'
            }
        }
    }

    post {
        always {
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
