def label = "mypod-${UUID.randomUUID().toString()}"

parameters {
	string(name: 'buid_id', defaultValue: "${env.BUILD_NUMBER}", description: 'What should I say?')
  }

podTemplate(label:label,cloud: "kubernetes",
    containers: [
    	containerTemplate(name: 'maven', image: 'maven:3.8.4-jdk-8', command: 'sleep', args: '99d',envVars: [envVar(key:"BUILD_NUMBER",value:"${env.BUILD_NUMBER}")]),
    	containerTemplate(name: 'docker', image: 'docker', command: 'sleep', args: '99d')]
	    ,
    volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock'),
	     hostPathVolume(hostPath: '/usr/bin/kubectl', mountPath: '/usr/local/bin/kubectl')]
	   ) {
  node(label) {
		
		environment {
			//registry = "YourDockerhubAccount/YourRepository"
			//registryCredential = 'dockerhub_id'
			//dockerImage = ''
        		//JOB_NAME = "${JOB_NAME}".replace("-deploy", "")
       			 //REGISTRY = "my-docker-registry"
			//KUBECONFIG_CREDENTIAL_ID = 'kubeconfig-credentials-id'
			//BUILD_ID = "${env.BUILD_NUMBER}"
  	
		}
	  
	  
	  	stage('SCM') {
			
				git credentialsId: 'github', url: 'git@github.com:jackbauer123/mytest.git'
			
			script {
			    build_tag = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
			    if (env.BRANCH_NAME != 'master') {
				build_tag = "${build_tag}-${env.BUILD_NUMBER}"
			    }
			}
			
		}
	  	stage('Build jar') {
			container('maven') {
				sh 'mvn -B -ntp clean package -DskipTests'
			}
			
		}
	  
	  	stage('build account image') {
			  
				  container('docker'){
					  account = docker.build("jackbauer123/account:${build_tag}","./account")
				  }
			 
		}
	  
	  	storage('build storage image') {
			  
				  container('docker'){
					  storage = docker.build("jackbauer123/storage:${build_tag}","./storage")
				  }
			 
		 }
		  
		  
		  
		  stage('build order image') {
			  
				  container('docker'){
					  order = docker.build("jackbauer123/order:${build_tag}","./order")
				  }
			 
		  }
	  
	  
	   	stage('build logic image') {
			  
				  container('docker'){	
					  logic = docker.build("jackbauer123/logic:${build_tag}","./logic")
				  }
			
		  }
	  
		
	  
	  stage('Push image') {
			  container('docker'){
				docker.withRegistry('', 'hubdocker') {
							storage.push("${build_tag}")
						
					
					account.push("${build_tag}")
							
					
					order.push("${build_tag}")
						
					
					logic.push("${build_tag}")
							
				} 

			  }	  
		 
			 
		 
	  
	  }
	  
	  	stage('deploy'){
			
			
	  		container('maven') {
				
				withKubeConfig([credentialsId: 'kube2', serverUrl: 'https://10.168.1.199:6443']) {
				//sh 'curl -LO -o https://storage.googleapis.com/kubernetes-release/release/$KUBECTL_VERSION/bin/linux/amd64/kubectl'
					sh "sed -i 's/<BUILD_TAG>/${build_tag}/' account/account.yaml"
					//sh "sed -i 's/<BRANCH_NAME>/${env.BRANCH_NAME}/' account/account.yaml"
			      		sh 'kubectl apply -f account/account.yaml --record'
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

