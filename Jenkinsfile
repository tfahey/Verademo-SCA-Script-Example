pipeline {
   agent any

   stages {
      stage('Verify Branch') {
         steps {
            echo "$GIT_BRANCH"
         }
      }
      stage('Package') {
         steps {
            echo "package"
            sh (script: """
                pwd
                mvn -f pom.xml clean package
                pwd
            """)  
            }
      }
      stage('SCA shell script') {
         steps {
            withCredentials([string(credentialsId: 'SRCCLR_API_TOKEN', variable: 'SRCCLR_API_TOKEN')]) {
            echo "srcclr scan"
               sh (script: """
                  cd app
                  pwd
                  curl -sSL https://download.sourceclear.com/ci.sh | sh
                  cd ..
                  pwd
               """)
            }
         }
      }
   }
   post {
      always {
         archiveArtifacts artifacts: 'app/target/*.war', followSymlinks: false
      }
   }
}
