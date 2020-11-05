pipeline {
   agent none
   stages {
      stage('Source Code') {
         agent { label 'Test' }
         steps {
            git 'https://github.com/ashish822013/capstone2.git'
         }
      }
      stage('Build website') {
         agent { label 'Test' }
         steps {
            		sh "sudo docker stop mywebsiteapp 2> /dev/null || true"
			sh "sudo docker rm mywebsiteapp 2> /dev/null || true"
			sh "sudo docker rmi ayadav081116/mywebsiteapp 2> /dev/null || true"
                        sh "docker build -t ayadav081116/mywebsiteapp:$BUILD_NUMBER ."
			sh "docker run -d -p 82:80 --name=mywebsiteapp ayadav081116/mywebsiteapp:$BUILD_NUMBER"
         }
      }
	stage('Push to Docker hub'){ 
	agent { label 'Test' } 
        steps {
        	withDockerRegistry([ credentialsId: "dockerhub-id", url: "https://index.docker.io/v1/" ]){
		sh "sudo docker push ayadav081116/mywebsiteapp:$BUILD_NUMBER"}
	}
	}
        stage('Publish to Production') {
         agent { label 'Prod' }
         steps {
            git 'https://github.com/ashish822013/capstone2.git'
            sh "sudo docker stop mywebsiteapp 2> /dev/null || true"
            sh "sudo docker rm mywebsiteapp 2> /dev/null || true"
            sh "sudo docker run --name mywebsiteapp -itd -p 82:80 ayadav081116/mywebsiteapp:$BUILD_NUMBER"
         }
	  }
	}
}
