pipeline {
    
    agent { label 'slave' }
    stages {
                   
        stage ('Checkout') {
          steps {
            git 'https://github.com/dilipsun/addressbook.git'
          }
        }
        stage('Build') {
            
             steps {
               
               sh '/opt/apache-maven-3.5.4/bin/mvn clean package'
                junit '**/target/surefire-reports/TEST-*.xml'
                                archiveArtifacts artifacts: '**/target/*.war', fingerprint: true
            }
        }
        stage('Docker image') {
        
            steps {
                
                sh 'docker build -t dilipsun/addressbook$(git rev-parse HEAD):latest .'
            }
        }
      }
    }
        stage('Docker Deploy') {
      steps {
          sh 'docker run -itd -P dilipsun/addressbook$(git rev-parse HEAD):latest'
        }
      }
    
    }
    
}
