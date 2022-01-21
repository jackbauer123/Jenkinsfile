def label = "mypod-${UUID.randomUUID().toString()}"

podTemplate(label:label,cloud: "kubernetes",
    containers: [
    	containerTemplate(name: 'maven', image: 'maven:3.8.4-jdk-8', command: 'sleep', args: '99d'),
    	containerTemplate(name: 'docker', image: 'docker', command: 'sleep', args: '99d')]
	    ,
    volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock'),
	     hostPathVolume(hostPath: '/usr/bin/kubectl', mountPath: '/usr/local/bin/kubectl')]
	   ) {
  node(label) {
		
		environment {
			registry = "YourDockerhubAccount/YourRepository"
			registryCredential = 'dockerhub_id'
			dockerImage = ''
        		JOB_NAME = "${JOB_NAME}".replace("-deploy", "")
       			 REGISTRY = "my-docker-registry"
			KUBECONFIG_CREDENTIAL_ID = 'kubeconfig-credentials-id'
			BUILD_ID = "${env.BUILD_NUMBER}"
  	
		}
	  
	  
	  	stage('SCM') {
			
				git credentialsId: 'github', url: 'git@github.com:jackbauer123/mytest.git'
			
		}
	  	stage('Build jar') {
			container('maven') {
				sh 'mvn -B -ntp clean package -DskipTests'
			}
			
		}
	  
	  	stage('build account image') {
			  
				  container('docker'){
					  storage = docker.build("jackbauer123/storage:${env.BUILD_ID}","./account")
				  }
			 
		  }
	  
	  	//stage('build storage image') {
			  
		//		  container('docker'){
		//			  storage = docker.build("jackbauer123/storage:${env.BUILD_ID}","./storage")
		//		  }
			 
		  //}
		  
		  
		  
		   //stage('build order image') {
			  
			//	  container('docker'){
			//		  order = docker.build("jackbauer123/order:${env.BUILD_ID}","./order")
			//	  }
			 
		  //}
	  
	  
	   	//stage('build logic image') {
			  
		//		  container('docker'){	
		//			  logic = docker.build("jackbauer123/logic:${env.BUILD_ID}","./logic")
		//		  }
			
		 // }
	  
		
	  	stage('deploy'){
			sh 'echo ${storage}'
	  		container('maven') {
				environment {
					image_version = ${env.BUILD_ID}
				}
				withKubeConfig([credentialsId: 'kube2', serverUrl: 'https://10.168.1.199:6443']) {
				//sh 'curl -LO -o https://storage.googleapis.com/kubernetes-release/release/$KUBECTL_VERSION/bin/linux/amd64/kubectl'
			      		sh 'kubectl apply -f account/account.yaml'
		//			sh 'kubectl apply -f storage/storage.yaml'
		//			sh 'kubectl apply -f order/order.yaml'
		//			sh 'kubectl apply -f logic/logic.yaml'
			    }
			}	
			
			
			
			
			//kubernetesDeploy(kubeconfigId: 'kubeconfig-credentials-id',               // REQUIRED

			//		 configs: 'account/account.yaml', // REQUIRED
			//		 enableConfigSubstitution: false
					 //,

					 //secretNamespace: '<secret-namespace>',
					 //secretName: '<secret-name>',
					 //dockerCredentials: [
					//	[credentialsId: '<credentials-id-for-docker-hub>'],
					//	[credentialsId: '<credentials-id-for-other-private-registry>', url: '<registry-url>'],
					 //]
			//)
	  
	  	}
	  
	
	 

		
    }

}

