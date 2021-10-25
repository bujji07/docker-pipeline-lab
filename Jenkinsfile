pipeline {
    agent { label 'Java' }
    stages { 
        stage ("SCM Checkin-1") {
            steps {
              git branch: 'main', url: 'https://github.com/bujji07/SampleMaven.git'
            }
        }
        stage ("maven build") {
            tools {
                maven 'maven3.8.2'
                jdk 'JDK 1.8'
            }
            steps {
                sh "mvn package"
            }
	    input {
                 message 'Mvn Package is Successfull, I can Proceed'
            }
        }
        stage ("Build the Docker Image") {
            steps {
                sh "docker build . -t sahityanidimoru/tomcat-b15"
            }
        }
        stage ("Run the Container") {
            steps {
                sh "docker run -d -p 90:8080 --name cont01 sahityanidimoru/tomcat-b15"
            }
        }
      //  stage ("Check the webpage") {
      //      steps {
      //          sh "sleep 15"
      //          sh "curl http://192.168.118.105:90/mvn-hello-world"
      //      }
      //  }
        stage ("Push to Docker HUB") {
            steps {
		withDockerRegistry(credentialsId:'DockerHub', url: '') {
		// some block
			sh "docker push sahityanidimoru/tomcat-b15:latest"
		}
            }
        }
        stage ("Cleanup the previous Docker Image and container") {
            input {
                    message 'Can we Delete the Docker image and Container'
            }
            steps {
                sh "docker rm cont01 -f"
                sh "docker image rmi sahityanidimoru/tomcat-b15 -f"
		//sh "mvn clean"
            }
        }
    }
}
