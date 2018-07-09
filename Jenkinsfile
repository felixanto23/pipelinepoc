pipeline {
    
    agent { label '10.200.152.50' }
    stages {
                   
        stage ('Checkout') {
          steps {
            git 'https://github.com/dilipsun/addressbook.git'
          }
        }
        stage('Build') {
            
             steps {
               
               sh '/usr/local/apache-maven/bin/mvn clean package'
                junit '**/target/surefire-reports/TEST-*.xml'
                                archiveArtifacts artifacts: '**/target/*.war', fingerprint: true
            }
        }
        stage('Docker image') {
        
            steps {
                
                sh 'docker build -t maven-sample:latest .'
            }
		}
        stage('Docker Push') {
			steps {
				sh "docker login -u admin -p admin 10.200.152.50:8081"
				sh "docker tag maven-sample 10.200.152.50:8081/docker-repo/maven-sample:1.0"
				sh "docker push 10.200.152.50:8081/docker-repo/maven-sample:1.0"
			}
		}
    
        stage('Docker Deploy') {
			steps {
				sh 'docker run -itd -P dilipsun/addressbook$(git rev-parse HEAD):latest'
			}
		}
	}
}
