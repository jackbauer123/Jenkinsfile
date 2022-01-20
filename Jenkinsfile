def label = "mypod-${UUID.randomUUID().toString()}"

podTemplate(label:label,cloud: "kubernetes",
    containers: [
    	containerTemplate(name: 'maven', image: 'maven:3.8.4-jdk-8', command: 'sleep', args: '99d'),
    	containerTemplate(name: 'docker', image: 'docker', command: 'sleep', args: '99d')]
	    ,
    volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')]
	   ) {
  node(label) {
		
		environment {
			registry = "YourDockerhubAccount/YourRepository"
			registryCredential = 'dockerhub_id'
			dockerImage = ''
		}
		stage('SCM') {
			//dir('/tmp'){
				git credentialsId: 'github', url: 'git@github.com:jackbauer123/mytest.git'
			//}
		}

		stage('Build') {
			container('maven') {
				sh 'mvn -B -ntp clean package -DskipTests'
			}
			
		}
	  
	 
		 
	  	
			 stage('build storage image') {
			  //dir('/tmp'){
				  container('docker'){
					  storage = docker.build("jackbauer123/storage:${env.BUILD_ID}","./storage")
				  }
			 // }
		  }
	  
	   stage('build account image') {
			  //dir('/tmp'){
				  container('docker'){
					  account = docker.build("jackbauer123/account:${env.BUILD_ID}","./account")
				  }
			 // }
		  }
	  
	   stage('build order image') {
			  //dir('/tmp'){
				  container('docker'){
					  order = docker.build("jackbauer123/order:${env.BUILD_ID}","./order")
				  }
			 // }
		  }
	  
	  
	   stage('build logic image') {
			  //dir('/tmp'){
				  container('docker'){
					  logic = docker.build("jackbauer123/logic:${env.BUILD_ID}","./logic")
				  }
			 // }
		  }
	  
	  
	  
		  stage('Push image') {
			  container('docker'){
				docker.withRegistry('', 'hubdocker') {
							storage.push("${env.BUILD_NUMBER}")
							storage.push("latest")
					
					account.push("${env.BUILD_NUMBER}")
							account.push("latest")
					
					order.push("${env.BUILD_NUMBER}")
							order.push("latest")
					
					logic.push("${env.BUILD_NUMBER}")
							logic.push("latest")
				} 

			  }	  
		 
			 
		 
	  
	  }
	 
		  
	  
	  	stage('deploy account'){
	  
			kubernetesDeploy(kubeconfigId: 'kubeconfig-credentials-id',               // REQUIRED

					 configs: 'account/account.yaml', // REQUIRED
					 enableConfigSubstitution: false
					 //,

					 //secretNamespace: '<secret-namespace>',
					 //secretName: '<secret-name>',
					 //dockerCredentials: [
					//	[credentialsId: '<credentials-id-for-docker-hub>'],
					//	[credentialsId: '<credentials-id-for-other-private-registry>', url: '<registry-url>'],
					 //]
			)
	  
	  	}
	  
	  stage('deploy storage'){
	  
			kubernetesDeploy(kubeconfigId: 'kubeconfig-credentials-id',               // REQUIRED

					 configs: 'storage.yaml', // REQUIRED
					 enableConfigSubstitution: false
					 //,

					 //secretNamespace: '<secret-namespace>',
					 //secretName: '<secret-name>',
					 //dockerCredentials: [
					//	[credentialsId: '<credentials-id-for-docker-hub>'],
					//	[credentialsId: '<credentials-id-for-other-private-registry>', url: '<registry-url>'],
					 //]
			)
	  
	  	}	
	  
	  stage('deploy order'){
	  
			kubernetesDeploy(kubeconfigId: 'kubeconfig-credentials-id',               // REQUIRED

					 configs: 'order.yaml', // REQUIRED
					 enableConfigSubstitution: false
					 //,

					 //secretNamespace: '<secret-namespace>',
					 //secretName: '<secret-name>',
					 //dockerCredentials: [
					//	[credentialsId: '<credentials-id-for-docker-hub>'],
					//	[credentialsId: '<credentials-id-for-other-private-registry>', url: '<registry-url>'],
					 //]
			)
	  
	  	}	
	  
	  
	  stage('deploy logic'){
	  
			kubernetesDeploy(kubeconfigId: 'kubeconfig-credentials-id',               // REQUIRED

					 configs: 'logic.yaml', // REQUIRED
					 enableConfigSubstitution: false
					 //,

					 //secretNamespace: '<secret-namespace>',
					 //secretName: '<secret-name>',
					 //dockerCredentials: [
					//	[credentialsId: '<credentials-id-for-docker-hub>'],
					//	[credentialsId: '<credentials-id-for-other-private-registry>', url: '<registry-url>'],
					 //]
			)
	  
	  	}	
	  
	 

		
    }

}

