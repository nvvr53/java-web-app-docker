node {
    
	def buildNumber = BUILD_NUMBER
    stage("Git Clone"){
        
        git url: 'https://github.com/nvvr53/java-web-app-docker.git', branch: 'master'
    }
    
    stage("Maven Clean Package"){
        
        def mavenHome= tool name: "Maven",type: "maven"
         sh "${mavenHome}/bin/mvn clean package"
    }
    
    stage("Build Docker Image"){
        
        sh "docker build -t dockernvvr/java-web-docker:${buildNumber} ."
    }
    
    stage("Docker Login And Pushing Image"){
        withCredentials([string(credentialsId: 'DockerPassword', variable: 'DockerPassword')]){
            sh "docker login -u dockernvvr -p ${DockerPassword}"
        }
        
            sh "docker push dockernvvr/java-web-docker "
    }
    stage("Deploy Application as Docker Container in DeploymentServer"){
        
     sshagent(['Deploy_Server']) {
          
          sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.43.201 docker rm -f javawebappcontainer || true"
          
          sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.43.201 docker run -d -p 8080:8080 --name javawebappcontainer dockernvvr/java-web-docker"
      }
        
    }
    

}
