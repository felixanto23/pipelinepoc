pipeline {
	
    agent { label 'slavenode' }
    stages {
                   
        stage ('Checkout Dockerfile') {
          steps {
            git 'https://github.com/dilipsun/addressbook.git'
          }
        }
        stage('Build and Upload') {
            
             steps {
               
               sh '/usr/local/apache-maven/bin/mvn clean package'
                junit '**/target/surefire-reports/TEST-*.xml'
                                archiveArtifacts artifacts: '**/target/*.war', fingerprint: true
		sh 'curl -u admin:admin -X PUT "http://10.200.152.50:8081/artifactory/build-artifacts/maven-sample/" -T **/target/*.war'
            }
        }
        stage('Docker Build') {
        
            steps {
                
                sh 'docker build -t maven-sample:latest .'
            }
		}
	stage('Docker Deploy') {
			steps {
				sh 'docker run -itd -P maven-sample:latest'
			}
		}
        stage('Docker Push') {
			steps {
				sh "docker login -u admin -p admin 10.200.152.50:8081"
				sh "docker tag maven-sample 10.200.152.50:8081/docker-repo/maven-sample:1.0"
				sh "docker push 10.200.152.50:8081/docker-repo/maven-sample:1.0"
			}
		}
	}
}
