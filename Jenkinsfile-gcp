pipeline {

  agent any
  

  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/milanoo7/hellowhale.git', branch:'master'
      }
    }
    
      stage("Build image") {
            steps {
                script {
                    myapp = docker.build("milan2312/hellowhale:${env.BUILD_ID}")
                }
            }
        }
	stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }
	
   	 stage("Deploy To Kuberates Cluster"){
		 steps{
      			  withCredentials([file(credentialsId: 'demo-key', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {
         			sh "gcloud auth activate-service-account --key-file=${GOOGLE_APPLICATION_CREDENTIALS}"
         			sh "gcloud config set project mssdevops-284216"
				 sh "gcloud config set compute/zone us-central1-c"
				 sh "gcloud config set compute/region us-central1"
				 sh "gcloud container clusters get-credentials cluster-1 --zone us-central1-c --project mssdevops-284216"
				 sh "sed -i -e 's,image_to_be_deployed,'milan2312/hellowhale:${BUILD_ID}',g' hellowhale.yml"
				 sh "kubectl apply -f hellowhale.yml"
			  }
			 
        }
	 }

   
  }

}
