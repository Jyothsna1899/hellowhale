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
	
      stage("Deploy App") {
	      steps{
		       sh "sed -i -e 's,image_to_be_deployed,'milan2312/hellowhale:${env.BUILD_ID}',g' hellowhale.yml"
	               sh "export KUBECONFIG=/etc/kubernetes/admin.conf && kubectl apply -f hellowhale.yml"
		      
       }
	      
    }

   
  }

}
