node {
    def app

    stage('Clone Repository') {
      

        checkout scm
    }

  stage('Build Image') {
       app = docker.build("prajvalgh/javaimageone")
    }

  stage('Push Image') {
        
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BUILD_NUMBER}")
        }
  }

	stage('Deploy')	{
		
			def dockerImage = docker.image('prajvalgh/assignmenttwo:${env.BUILD_NUMBER}')
			dockerImage.run('-d -p 8081:8080 --name app')	

		}

	}
