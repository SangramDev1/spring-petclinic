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
        stage('source code pulling') {
            steps {
                git branch: 'main', url: 'https://github.com/SangramDev1/spring-petclinic.git'
            }
        }

        stage('building the code') {
            steps {
                sh '''
                    export M2_HOME='/opt/apache-maven-3.9.8'
                    export PATH="$M2_HOME/bin:$PATH"
                    mvn package
                '''    
            }
        }

       stage("Archiving the Artifacts and Test Results") {
            steps {
                junit '**/surefire-reports/*.xml'
                archiveArtifacts artifacts: '**/*.war', followSymlinks: false
            }
        }
    }

   post {
        success {
            echo 'success'          
        }
        failure { 
            echo 'unsuccess'
        }
    }
}
