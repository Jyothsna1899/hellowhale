# hellowhale
Simple Docker Demo App/.///


on Azure this is way we can connect K8 
  /** stage("Deploy To Kuberates Cluster"){
       kubernetesDeploy(
         configs: 'springBootMongo.yml', 
         kubeconfigId: 'mykubeconfig',
         enableConfigSubstitution: true
        )
     }**/
    Or second way is
    ////////////////////////////
  /**  
     
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "hellowhale.yml", kubeconfigId: "mykubeconfig")
		  
		     stage('Deploy App') {
      steps {
          sh "sed -i -e 's,image_to_be_deployed,'milan2312/hellowhale:${env.BUILD_ID}',g' hellowhale.yml"
	  sh "export KUBECONFIG=/etc/kubernetes/admin.conf && kubectl apply -f hellowhale.yml"
      }
        }
      }
    }

  }

} **/

/////// On GCP k8 cluster following steps need to do to connect Jenkins and K8 /////


/** 
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



**/
