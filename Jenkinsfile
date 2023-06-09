node {
    def app

    stage('Clone Repository') {
      

        checkout scm
    }

  stage('Build Image') {
  
       app = docker.build("prajvalgh/assignmenttwo/target")
    }

  stage('Push Image') {
        
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BUILD_NUMBER}")
        }
    }


 stage('Update GIT') {
            script {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                      
                        sh "git config user.email prajvalgh12@gmail.com"
                        sh "git config user.name prajvalgh"
                      
                        sh "cat deployment.yaml"
                        sh "sed -i 's+prajvalgh/assignmenttwo.*+prajvalgh/assignmenttwo:${DOCKERTAG}+g' deployment.yaml"
                        sh "cat deployment.yaml"
                        sh "git add ."
                        sh "git commit -m 'By Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
			sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/Java-Web-Apps.git HEAD:main"
      }
    }
  }
}
}
