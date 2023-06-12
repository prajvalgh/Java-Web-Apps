node {
    def app

    stage('Clone Repository') {
      

        checkout scm
    }

	stage('Update GIT') {
            script {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                      
                        sh "git config user.email prajvalgh12@gmail.com"
                        sh "git config user.name prajvalgh"
                      
                // Build your Java application
                // For example, using Maven
		
                sh 'mvn clean install'
		sh 'mvn package'
            }
        }

  stage('Build Image') {
  
       app = docker.build("prajvalgh/assignmenttwo")
    }

  stage('Push Image') {
        
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BUILD_NUMBER}")
        }
  }

	stage('Deploy')	{
		
			def dockerImage = docker.image('prajvalgh/assignmenttwo:1')
			dockerImage.run('-d -p 8081:8080 --name app')	

		}

	}
