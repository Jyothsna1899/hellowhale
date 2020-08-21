currentBuild.displayName="movies-app-#"+currentBuild.number
pipeline {
    agent any
     stages{
        stage('Build Docker Image'){
            steps{
                sh "sudo docker build . -t srinivasareddy4218/hellowhale:latest"
            }
        }
        stage('DockerHub Push'){
            steps{
               withCredentials([string(credentialsId: 'sree-docker', variable: 'sample')]) {
                    sh "sudo docker login -u srinivasareddy4218 -p ${sample}"
                    sh "sudo docker push srinivasareddy4218/movies-app:latest"
                }
            }
        }
        
         stage("Deploy To Kuberates Cluster"){
		steps{
          sshagent(['sshkey']){
	   	  
          /**frontend **/			
	  sh "sed -i -e 's,image_to_be_deployed,'srinivasareddy4218/movies-app:latest',g' hellowhale.yaml"
    kubectl apply -f hellowhale.yml -n sree
    }
   }
  }
 }
} 
